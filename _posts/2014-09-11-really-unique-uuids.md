---
id: 1168
title: Really unique UUIDs
date: 2014-09-11T10:41:07-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=1168
permalink: /blog/2014/09/really-unique-uuids/
categories:
  - Software Development
tags:
  - Cassandra
  - Scala
---
Recently we encountered a problem with duplicate time UUIDs while loading a lot of data into [Cassandra](http://cassandra.apache.org/). Duplicates are not normally a problem with UUIDs but occasionally you need to generate time UUIDS from a low resolution clock and/or load a lot of data really fast. In these situations you can overwhelm the ability of â€œcorrectâ€ (to the spec) implementations of time UUID generation to create truly unique id.

The problem is that [version 1 UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier#Version_1_.28MAC_address.29) use a lot of their bits to store the [MAC address](http://en.wikipedia.org/wiki/MAC_address), leaving only enough space for a 100 nanosecond resolution timestamp and a small sequence number. That works fine is great if a bunch of different machine are generating UUIDs fairly slowly but if you have a small number of machine generating them as fast as possible it is just not good enough.

The solution we came up with was to replace the MAC address and clock sequence in the UUID with a large random number. Doing so adds a lot of entropy to the ID while maintaining its time UUID structure. Building UUIDs requires some bit twiddling but is not difficult.

The exact structure of a time UUID is described in [RFC 4122](http://tools.ietf.org/html/rfc4122#section-4.1.2). Creating a UUID in Scala (and Java) requires two longs, the most significant bits and least significant bits. The most significant bits contain the timestamp and a version identifier. The least significant bits usually contains the MAC address and a clock sequence but we want something a bit more unique.

    new UUID(timeAndVersionFields(time), randomClockSeqAndNodeFields)

`timeAndVersionFields` constructs the time and version fields of the UUID from the milliseconds since Unix epoch, we just have to rearrange the bits of the time a little:

    def timeAndVersionFields(time: Long) = {
      var msb: Long = 0L
      msb |= (0x00000000ffffffffL & time) << 32
      msb |= (0x0000ffff00000000L & time) >>> 16
      msb |= (0x0fff000000000000L & time) >>> 48
      msb |= 0x0000000000001000L // identify as a version 1 uuid
      msb
    }

That the same as every other time UUID generator. For the least significant bits we need to do things a little differently.

    def rand = new Random()
    
    def randomClockSeqAndNodeFields = {
      var lsb: Long = 0
      lsb |= 0x8000000000000000L // variant (2 bits)
      lsb |= ( rand.synchronized { rand.nextLong } & 0x3FFFFFFFFFFFFFFFL)
      lsb
    }

`randomClockSeqAndNodeFields` generates a big random number and uses that for the fields that normally contains the MAC address and clock sequence. This provides a great deal of protection against duplicate UUIDs even when generating many time UUIDs very quickly on a small number of machines.