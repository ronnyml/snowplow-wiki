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
    - 4.2.1 [`Start()`](#e-start)
    - 4.2.2 [`Stop()`](#e-stop)
    - 4.2.3 [`Add(TrackerPayload)`](#e-add)
- 5. [Subject](#subject)
  - 5.1 [`setUserId`](#set-user-id)
  - 5.2 [`setResolutionWithWidth`](#set-res)
  - 5.3 [`setViewPortWithWidth`](#set-view-port)
  - 5.4 [`setColorDepth`](#set-color-depth)
  - 5.5 [`setTimezone`](#set-timezone)
  - 5.6 [`setLanguage`](#set-language)
  - 5.7 [`setIpAddress`](#set-ip-address)
  - 5.8 [`setUseragent`](#set-useragent)
  - 5.9 [`setNetworkUserId`](#set-nuid)
  - 5.10 [`setDomainUserId`](#set-duid)
- 6. [Session](#session)
  - 6.1 [Constructor](#s-constructor)
  - 6.2 [Functions](#s-functions)
    - 6.2.1 [`StartChecker()`](#s-start)
    - 6.2.2 [`StopChecker()`](#s-stop)
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

[snowplow-pong-dl]: http://
[base64]: https://en.wikipedia.org/wiki/Base64
