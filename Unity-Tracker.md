<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers) > Unity Tracker

This page refers to version 0.1.0 of the Snowplow Unity Tracker.

## Contents

- 1. [Overview](#overview)
  - 1.1 [Snowplow Pong Demo](#demo-app)
- 2. [Initialization](#init)
  - 2.1 [Importing the library](#importing)
  - 2.2 [Creating a tracker](#create-tracker)
- 3. [Tracker](#tracker)
  - 3.1 [Constructor](#t-constructor)
  - 3.2 [Functions](#t-functions)
    - 3.2.1 [`StartEventTracking()`](#t-start)
    - 3.2.2 [`StopEventTracking()`](#t-stop)
    - 3.2.3 [`Track(IEvent)`](#t-track)
- 4. [Emitter](#emitter)
  - 4.1 [Constructor](#e-constructor)
  - 4.2 [Functions](#e-functions)
    - 4.2.3 [`Add(TrackerPayload)`](#e-add)
- 5. [Subject](#subject)
  - 5.1 [`SetUserId`](#set-user-id)
  - 5.2 [`SetResolutionWithWidth`](#set-res)
  - 5.3 [`SetViewPortWithWidth`](#set-view-port)
  - 5.4 [`SetColorDepth`](#set-color-depth)
  - 5.5 [`SetTimezone`](#set-timezone)
  - 5.6 [`SetLanguage`](#set-language)
  - 5.7 [`SetIpAddress`](#set-ip-address)
  - 5.8 [`SetUseragent`](#set-useragent)
  - 5.9 [`SetNetworkUserId`](#set-nuid)
  - 5.10 [`SetDomainUserId`](#set-duid)
- 6. [Session](#session)
  - 6.1 [Constructor](#s-constructor)
  - 6.2 [Functions](#s-functions)
    - 6.2.3 [`GetSessionContext(string)`](#s-get-context)
    - 6.2.4 [`SetBackground(bool)`](#s-set-background)
- 7. [Event Tracking](#event-tracking)
  - 7.1 [Events Types](#event-types)
    - 7.1.1 [`PageView`](#et-page-view)
    - 7.1.2 [`ScreenView`](#et-screen-view)
    - 7.1.3 [`Structured`](#et-structured)
    - 7.1.4 [`Timing`](#et-timing)
    - 7.1.5 [`Unstructured`](#et-unstructured)
    - 7.1.6 [`EcommerceTransaction`](#et-ecomm)
      - 7.1.6.1 [`EcommerceTransactionItem`](#et-ecomm-item)
  - 7.2 [Custom Contexts](#context-types)
    - 7.2.1 [`DesktopContext`](#ct-desktop)
    - 7.2.2 [`MobileContext`](#ct-mobile)
    - 7.2.3 [`GeoLocationContext`](#ct-geo-location)
    - 7.2.4 [`SessionContext`](#ct-session)
    - 7.2.5 [`GenericContext`](#ct-generic)
- 8. [Utilities](#utilities)
  - 8.1 [Log](#log)
  - 8.2 [ConcurrentQueue](#concurrent)

<a name="overview" />
## 1. Overview

The [Snowplow Unity Tracker](https://github.com/snowplow/snowplow-unity-tracker) allows you to track Snowplow events from your Unity games and apps.

[Back to top](#top)

<a name="demo-app" />
### 1.1 Snowplow Pong Demo

To see the Tracker in action you can download our demonstration game Snowplow Pong from [here][snowplow-pong-dl].  You will need to set a valid collector endpoint on the Settings Page to see the events being generated.

[Back to top](#top)

<a name="init" />
## 2. Initialization

Assuming you have completed the [[Unity Tracker Setup]] for your project, you are now ready to initialize the Unity Tracker.

[Back to top](#top)

<a name="importing" />
### 2.1 Importing the library

Add the following `using` lines to the top of your `.cs` scripts to access the Tracker:

```csharp
using SnowplowTracker;
using SnowplowTracker.Emitters;
using SnowplowTracker.Enums;
using SnowplowTracker.Events;
```

You should now be able to setup the Tracker!

[Back to top](#top)

<a name="create-tracker" />
### 2.2 Creating a tracker

To instantiate a Tracker in your code (can be global or local to the process being tracked) simply instantiate the Tracker interface with the following:

```csharp
// Create Emitter and Tracker
IEmitter e1 = new AsyncEmitter ("com.collector.acme")
Tracker t1 = new Tracker(e1, "Namespace", "AppId");

// Start the Tracker
t1.StartEventTracking();
```

This is the simplest possible Tracker creation available.  For more information please review the [Tracker](#tracker) and [Emitter](#emitter) specific sections.

[Back to top](#top)

<a name="tracker" />
## 3. Tracker

The Tracker object is responsible for co-ordinating the saving and sending of events as well as managing the optional Session object.

[Back to top](#top)

<a name="t-constructor" />
### 3.1 Constructor

| **Argument Name**  | **Description**                              | **Required?**  | **Default**   |
|-------------------:|:---------------------------------------------|:---------------|:--------------|
| `emitter`          | The Emitter object you create                | Yes            | Null          |
| `trackerNamespace` | The name of the tracker instance             | Yes            | Null          |
| `appId`            | The application ID                           | Yes            | Null          |
| `subject`          | The Subject that defines a user              | No             | Null          |
| `session`          | The Session object you create                | No             | Null          |
| `platform`         | The device the Tracker is running on         | No             | Mobile        |
| `base64Encoded`    | If we [base 64 encode][base64] json values   | No             | True          |

A full Tracker construction should look like the following:

```csharp
IEmitter e1 = new AsyncEmitter ("com.collector.acme")
Subject subject = new Subject();
Session session = new Session();
Tracker t1 = new Tracker(e1, "Namespace", "AppId", subject, session, DevicePlatforms.Desktop, true);
```

All of these variables can be altered after creation with the accompanying `tracker.SetXXX()` function.

[Back to top](#top)

<a name="t-functions" />
### 3.2 Functions

The Tracker also contains several critical functions that must be used to start Tracking.

[Back to top](#top)

<a name="t-start" />
#### 3.2.1 `StartEventTracking()`

This function must be called before any events will start being stored or sent.  This is due to the fact that we do not want to start any background processing from the constructors so it is left up to the developer to choose when to start everything up.

This function:

* Starts the background emitter thread
* Starts the background event processor thread
* Starts the background session check timer (Optional)

Once this is run everything should be in place for asynchronous event tracking.

[Back to top](#top)

<a name="t-stop" />
#### 3.2.2 `StopEventTracking()`

If you need to temporarily halt the Tracker from tracking events you can run this function.  This will bring all event processing, sending and collection to a halt and nothing will be started again until `StartEventTracking()` is fired again.

[Back to top](#top)

<a name="t-track" />
#### 3.2.3 `Track(IEvent)`

This is the function used for Tracking all events.  You can pass any of [these](#event-types) event types to this function.

```csharp
tracker.Track(IEvent newEvent);
```

[Back to top](#top)

<a name="emitter" />
## 4. Emitter

The Emitter object is responsible for sending and storing all events.

[Back to top](#top)

<a name="e-constructor" />
### 4.1 Constructor

| **Argument Name**  | **Description**                              | **Required?**  | **Default**   |
|-------------------:|:---------------------------------------------|:---------------|:--------------|
| `endpoint`         | The collector uri the emitter should use     | Yes            | Null          |
| `protocol`         | The request Protocol (HTTP or HTTPS)         | No             | HTTP          |
| `method`           | The HTTP Method (GET or POST)                | No             | POST          |
| `sendLimit`        | The amount of events to send at a time       | No             | 500           |
| `byteLimitGet`     | The byte limit for a GET request             | No             | 52000         |
| `byteLimitPost`    | The byte limit for a POST request            | No             | 52000         |

A full Emitter construction should look like the following:

```csharp
IEmitter e1 = new AsyncEmitter ("com.collector.acme", HttpProtocol.HTTPS, HttpMethod.GET, 50, 30000, 30000);
```

All of these variables can be altered after creation with the accompanying `emitter.SetXXX()` function.  However do be aware that multiple threads will be accessing these variables so to be safe always shut the Tracker down using `StopEventTracking()` before ammending anything.

[Back to top](#top)

<a name="e-functions" />
### 4.2 Functions

The Emitter also contains several extra functions.

[Back to top](#top)

<a name="e-add" />
#### 4.2.3 `Add(TrackerPayload)`

This is the public function used by the Tracker to add and send events from the emitter.  If for whatever reason the Tracker needs to be bypassed you can add a TrackerPayload object directly to the Emitter.

[Back to top](#top)

<a name="subject" />
## 5. Subject

You may have additional information about your application's environment, current user and so on, which you want to send to Snowplow with each event.

The Subject class has a set of `Set...()` methods to attach extra data relating to the user to all tracked events:

* [`SetUserId`](#set-user-id)
* [`SetScreenResolution`](#set-screen-resolution)
* [`SetViewport`](#set-viewport)
* [`SetColorDepth`](#set-color-depth)
* [`SetTimezone`](#set-timezone)
* [`SetLanguage`](#set-lang)
* [`SetIpAddress`](#set-ip-address)
* [`SetUseragent`](#set-user-agent)
* [`SetNetworkUserId`](#set-network-user-id)
* [`SetDomainUserId`](#set-domain-user-id)

Here are some examples:

```csharp
Subject s1 = new Subject();
s1.SetUserId("Kevin Gleason"); 
s1.SetLanguage("en-gb");
s1.SetScreenResolution(1920, 1080);
```

After that, you can add your Subject to your Tracker like so:

```csharp
Tracker t1 = new Tracker(..., s1)

// OR

t1.SetSubject(s1);
```

[Back to top](#top)

<a name="set-user-id" />
### 5.1 Set user ID with `SetUserId`

You can set the user ID to any string:

```csharp
s1.SetUserId( "{{USER ID}}" )
```

Example:

```csharp
s1.SetUserId("alexd")
```

[Back to top](#top)

<a name="set-screen-resolution" />
### 5.2 Set screen resolution with `SetScreenResolution`

If your C# code has access to the device's screen resolution, then you can pass this in to Snowplow too:

```csharp
t1.SetScreenResolution( {{WIDTH}}, {{HEIGHT}} )
```

Both numbers should be positive integers; note the order is width followed by height. Example:

```csharp
t1.SetScreenResolution(1366, 768)
```

[Back to top](#top)

<a name="set-viewport-dimensions" />
### 5.3 Set viewport dimensions with `SetViewport`

If your C# code has access to the viewport dimensions, then you can pass this in to Snowplow too:

```csharp
s.SetViewport( {{WIDTH}}, {{HEIGHT}} )
```

Both numbers should be positive integers; note the order is width followed by height. Example:

```csharp
s.SetViewport(300, 200)
```

[Back to top](#top)

<a name="set-color-depth" />
### 5.4 Set color depth with `SetColorDepth`

If your C# code has access to the bit depth of the device's color palette for displaying images, then you can pass this in to Snowplow too:

```csharp
s.SetColorDepth( {{BITS PER PIXEL}} )
```

The number should be a positive integer, in bits per pixel. Example:

```csharp
s.SetColorDepth(32)
```

[Back to top](#top)

<a name="set-timezone" />
### 5.5 Set timezone with `SetTimezone`

This method lets you pass a user's timezone in to Snowplow:

```csharp
s.SetTimezone( {{TIMEZONE}} )
```

The timezone should be a string:

```csharp
s.SetTimezone("Europe/London")
```

[Back to top](#top)

<a name="set-lang" />
### 5.6 Set the language with `SetLanguage`

This method lets you pass a user's language in to Snowplow:

```csharp
s.SetLanguage( {{LANGUAGE}} )
```

The language should be a string:

```csharp
s.SetLanguage('en')
```

[Back to top](#top)

<a name="set-ip-address" />
### 5.7 `SetIpAddress`

This method lets you pass a user's IP Address in to Snowplow:

```csharp
SetIpAddress( {{IP ADDRESS}} )
```

The IP address should be a string:

```csharp
subj.SetIpAddress("127.0.0.1");
```

[Back to top](#top)

<a name="set-user-agent" />
### 5.8 `SetUseragent`

This method lets you pass a useragent in to Snowplow:

```csharp
SetUseragent( {{USERAGENT}} )
```

The useragent should be a string:

```csharp
subj.SetUseragent("Agent Smith");
```

[Back to top](#top)

<a name="set-network-user-id" />
### 5.9 `SetNetworkUserId`

This method lets you pass a Network User ID in to Snowplow:

```csharp
SetNetworkUserId( {{NUID}} )
```

The network user id should be a string:

```csharp
subj.SetNetworkUserId("network-id");
```

[Back to top](#top)

<a name="set-domain-user-id" />
### 5.10 `SetDomainUserId`

This method lets you pass a Domain User ID in to Snowplow:

```csharp
SetDomainUserId( {{DUID}} )
```

The domain user id should be a string:

```csharp
subj.SetDomainUserId("domain-id");
```

[Back to top](#top)

[snowplow-pong-dl]: http://
[base64]: https://en.wikipedia.org/wiki/Base64
