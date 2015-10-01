[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers)

[[https://d3i6fms1cm1j0i.cloudfront.net/github-wiki/images/snowplow-architecture-1a-trackers.png]]

## Overview

**Trackers** are client- or server-side libraries which track customer behaviour by sending Snowplow events to a [Collector](collectors).

## Tracker documentation

### Trackers

* [JavaScript Tracker](Javascript-Tracker) - for tracking user activity on websites and webapps
* [Pixel Tracker](pixel-tracker) - a pixel tracker for web environments where JavaScript is not available
* [[Python Tracker]] - for tracking events in your Python-based applications, games or Python web servers/frameworks
* [[iOS Tracker]] - for tracking events in your iOS apps and games
* [[.NET Tracker]] - for tracking events in your .NET applications
* [[Ruby Tracker]] - for tracking events in your Ruby and Rails apps and gems
* [[Java Tracker]] - for tracking events in your Java-based desktop and server apps, servlets and games
* [[Lua Tracker]] - track events in your Lua-based applications, Lua web servers/frameworks, or from the Lua scripting layer within your games or apps
* [[Arduino Tracker]] - for tracking events from an IP-connected Arduino board
* [[ActionScript3 Tracker]] - for tracking events in your Flash Player 9+, Flash Lite 4 and AIR games, apps and widgets

For other trackers (e.g. iOS, Android) and their approximate timelines, please see the [Product roadmap](Product-roadmap).

### Protocol

* [Snowplow Tracker Protocol](snowplow-tracker-protocol) - the protocol implemented by all trackers
