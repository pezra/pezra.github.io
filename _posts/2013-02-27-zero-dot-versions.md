---
id: 946
title: Zero dot versions
date: 2013-02-27T06:30:00-07:00
author: Peter Williams
layout: post
guid: https://barelyenough.org/?p=946
permalink: /blog/2013/02/zero-dot-versions/
geo_latitude:
  - "39.8837641"
  - "39.8837641"
geo_longitude:
  - "-104.9880395"
  - "-104.9880395"
geo_public:
  - "1"
  - "1"
categories:
  - Software Development
---
Dear library developers, please knock that shit off immediately.

We all seem to accept the wisdom of [semantic versioning](http://semver.org) these days (thank goodness). Somehow, though, it has not occurred to many library developers that locking the first slot of the version to 0 means you give up all those benefits. Incrementing the first slot is how clients are informed of incompatible changes. If you never change the first slot you necessarily stop communicating this information.

If you have released you library to the rest of the world it should not have a &#8216;0.&#8217; version. Period. If you think most people probably should not be using your library, add a pre-release tag to the version. If you want to tell the world that the API is likely to change use the docs/readme, that is why it exists. Or you could skip telling us altogether because everybody already knows the API is likely to change. That is why we came up with semantic versioning in the first place.