<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers) > Scala Tracker

**Warning: the Scala Tracker has not yet been released.**

<a name="overview" />
## 1. Overview

The Snowplow Scala Tracker allows you to track Snowplow events in your Scala apps and servers.

The tracker should be straightforward to use if you are comfortable with Scala development; any prior experience with Snowplow's [[Python Tracker]], [[JavaScript Tracker]], [[Lua Tracker]], Google Analytics or Mixpanel (which have similar APIs to Snowplow) is helpful but not necessary.

There are three main classes which the Scala Tracker uses: subjects, emitters, and trackers.

A subject represents a single user whose events are tracked, and holds data specific to that user.

A tracker always has one active subject at a time associated with it. The default subject only has "platform=server" configured, but you can replace it with a subject containing more data. The tracker constructs events with that subject and sends them to one or more emitters, which sends them on to a Snowplow collector.

<a name="init" />
## 2. Initialization

Assuming you have completed the [[Scala Tracker Setup]], you are ready to initialize the Scala Tracker.

```scala
import com.snowplowanalytics.snowplow.scalatracker._
import com.snowplowanalytics.snowplow.scalatracker.emitters._

val emitter1 = AsyncEmitter.createAndStart("mycollector.com")
val emitter2 = new SyncEmitter("myothercollector.com", port = 8080)
val tracker = new Tracker(List(emitter, emitter2), "mytrackername", "myapplicationid")
```

The above code:

* creates a non-blocking emitter, `emitter1`, which sends events to "mycollector.com" on the default port, port 80
* creates a blocking emitter, `emitter1`, which sends events to "myothercollector.com" on port 8080
* creates a tracker which can be used to send events to both emitters

You can configure a subject with extra data and attach it to the tracker so that the data will be attached to every event:

```scala
val subject = new Subject()
  .setUserId("user-00035")
  .setPlatform(Desktop)
tracker.setSubject(subject)
```

<a name="events" />
## 3. Sending events

Create a Snowplow unstructured event [self-describing JSON][self-describing-jsons] using the [json4s DSL][json4s-dsl]:

```scala
import org.json4s.JsonDSL._

val productViewEvent = SelfDescribingJson(
  "iglu:com.acme/product_view/jsonschema/1-0-0",
  ("userType" -> "tester") ~ ("sku" -> "0000013")
)
```

Send it using the `trackUnstructEvent` tracker method:

```scala
tracker.trackUnstructEvent(productViewEvent)
```

You can attach any number of custom contexts to an event:

```scala
val pageTypeContext = SelfDescribingJson(
  "iglu:com.acme/page_type/jsonschema/1-0-0",
  ("type" -> "promotional") ~ ("backgroundColor" -> "red")
)

val userContext = SelfDescribingJson(
  "iglu:com.acme/user/jsonschema/1-0-0",
  ("userType" -> "tester")
)

t.trackUnstructEvent(productViewEvent, List(pageTypeContext, userContext))
```

A timestamp will automatically be attached to every event. You can override this timestamp by providing your own. It should be in seconds since the Unix epoch:

```scala
tracker.trackUnstructEvent(productViewEvent, Nil, 1432806619L)
```

<a name="subject" />
## 4. Subject methods

A list of the methods used to add data to the Subject class.

<a name="set-platform" />
### 4.1 Set the platform with `setPlatform`

The default platform is `Server`. These are the available alternatives, all available in the package `com.snowplowanalytics.snowplow.scalatracker`:

```scala
Server
Web
Mobile
Desktop
Tv
Console
InternetOfThings
General
```

Example usage

```scala
subject.setPlatform(Tv)
```

<a name="set-user-id" />
### 4.2 Set the user ID with `setUserId`

You can make the user ID a string of your choice:

```scala
subject.setUserId('user-000563456')
```

<a name="set-screen-resolution" />
### 4.3 Set the screen resolution with `setScreenResolution`

If your Scala code has access to the device's screen resolution, you can pass it in to Snowplow. Both numbers should be positive integers; note the order is width followed by height. Example:

```scala
subject.setScreenResolution(1366, 768)
```

<a name="set-viewport" />
### 4.4 Set the viewport dimensions with `setViewport`

Similarly, you can pass the viewport dimensions in to Snowplow. Again, both numbers should be positive integers and the order is width followed by height. Example:

```scala
subject.setViewport(300, 200)
```

<a name="set-color-depth" />
### 4.5 Set the color depth with `setColorDepth`

If your Scala code has access to the bit depth of the device's color palette for displaying images, you can pass it in to Snowplow. The number should be a positive integer, in bits per pixel.

```scala
subject.setColorDepth(24)
```

<a name="set-timezone" />
### 4.6 Setting the timezone with `setTimezone`

If your Scala code has access to the timezone of the device, you can pass it in to Snowplow:

```scala
subject.setTimezone('Europe London')
```

<a name="set-language" />
### 4.7 Setting the language with `setLang`

You can set the language field like this:

```scala
subject.setLang('en')
```

<a name="set-ip-address" />
### 4.8 Setting the IP address with `setIpAddress`

If you have access to the user's IP address, you can set it like this:

```scala
subject.setIpAddresss('34.634.11.139')
```

<a name="set-useragent" />
### 4.9 Setting the useragent with `setUseragent`

If you have access to the user's useragent (sometimes called "browser string"), you can set it like this:

```scala
subject.setUseragent('Mozilla/5.0 (Windows NT 5.1; rv:24.0) Gecko/20100101 Firefox/24.0')
```

<a name="set-domain-user-id" />
### 4.10 Setting the domain user ID with `setDomainUserId`

The `domain_userid` field of the Snowplow event model corresponds to the ID stored in the first party cookie set by the Snowplow JavaScript Tracker. If you want to match up server-side events with client-side events, you can set the domain user ID for server-side events like this:

```scala
subject.setDomainUserId('c7aadf5c60a5dff9')
```

<a name="set-network-user-id" />
### 4.11 Setting the network user ID with `setNetworkUserId`

The `network_user_id` field of the Snowplow event model corresponds to the ID stored in the third party cookie set by the Snowplow Clojure Collector and Scala Stream Collector. You can set the network user ID for server-side events like this:

```scala
subject.setNetworkUserId('ecdff4d0-9175-40ac-a8bb-325c49733607')
```

[Back to top](#top)

[json4s]: https://github.com/json4s/json4s
[json4s-dsl]: https://github.com/json4s/json4s#dsl-rules

[self-describing-jsons]: https://github.com/snowplow/iglu/wiki/Self-describing-JSONs
