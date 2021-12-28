---
id: 1569
title: supervisor child startup in elixir
date: 2018-04-25T18:29:20-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=1569
permalink: /blog/2018/04/supervisor-child-startup-in-elixir/
categories:
  - Software Development
tags:
  - elixir
---
Fact: [Supervisors](https://hexdocs.pm/elixir/Supervisor.html#module-start_link-2-init-2-and-strategies) **fully** serialize the startup of their children. Each child's `init/1` function runs to completion before the supervisor starts the next child.

For example, given modules `ModuleA` and `ModuleB` that implement both the `init/1` callback and this supervisor:

    Supervisor.start_link([
      ModuleA,
      ModuleB
    ], strategy: :one_for_one)

The `ModuleA` child process will be started and its `init/1` function will run to completion before the `ModuleB` child process starts. This is true regardless of the strategy used.

## so what? {#so_what}

This fact can often simplify application startup and error handling scenarios. For example, consider a situation where several processes need a token to interact with an API. The application must acquire this token on startup. If the token expires or the server revokes the token the application must acquire a brand new token and distribute it to all the processes.

We can implement that behavior simply and easily using serialization and appropriate crash handling strategy of a supervisor.

    defmodule MyApi.TokenManager do
      use GenServer

      def init(_) do
        token = fetch_token_from_api!()
        {:ok, token}
      end

      def handle_call(:get_token, _from, token) do
        {:reply, token, token}
      end
    end

    defmodule MyApi.ThingDoer do
      use GenServer

      def handle_call(:do_thing, _from, _state) do
        token = MyApi.TokenManager.get_token()

        # do stuff with token; crash if it doesn't work
      en
    end

    defmodule MyApi.OtherThingDoer do
      use GenServer

      def handle_call(:do_other_thing, _from, _state) do
        token = MyApi.TokenManager.get_token()

        # do stuff with token; crash if it doesn't work
      en
    end

    Supervisor.start_link([
      MyApi.TokenManager,
      MyApi.ThingDoer,
      MyApi.OtherThingDoer
    ], strategy: :one_for_all)

In this example `MyApi.TokenManager.init/1` acquires the token before returning. That means the token is ready by the time `MyApi.ThingDoer` and `MyApi.OtherThingDoer` start. If at any point to the API server revokes the token, or it expires, the next thing doer to try to use it can just crash. That crash will cause the supervisor to shutdown the remaining children and restart them all beginning with `MyApi.TokenManager` which will acquire a new, working token.

With this approach `MyApi.ThingDoer` and `MyApi.OtherThingDoer` don't need any specific error handling code around token management. The removal of situation-specific error handling logic makes them simpler and more reliable.
