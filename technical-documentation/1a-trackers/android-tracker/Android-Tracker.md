<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers) > Android Tracker

This page refers to version 0.3.0 of the Snowplow Android Tracker. [UNRELEASED]

* [Java v0.6.* and Android v0.2.*][java-0.6]
* [Java v0.5.* and Android v0.1.*][java-0.5]

## Contents

- 1. [Overview](#overview)  
- 2. [Initialization](#init)
  - 2.1 [Importing the module](#importing)
  - 2.2 [Creating a tracker](#create-tracker)
    - 2.2.1 [`emitter`](#emitter)
    - 2.2.2 [`subject`](#subject)
    - 2.2.3 [`namespace`](#namespace)
    - 2.2.4 [`appId`](#app-id)
    - 2.2.5 [`base64`](#base64)
    - 2.2.6 [`platform`](#platform)
    - 2.2.7 [`setPlatform`](#set-platform)
    - 2.2.8 [`setSubject`](#set-subject)
    - 2.2.9 [`setEmitter`](#set-emitter)
- 3. [Adding extra data: the Subject class](#add-data)
  - 3.1 [`setUserId`](#set-user-id)
  - 3.2 [`setScreenResolution`](#set-screen-resolution)
  - 3.3 [`setViewport`](#set-viewport-dimensions)
  - 3.4 [`setColorDepth`](#set-color-depth)
  - 3.5 [`setTimezone`](#set-timezone)
  - 3.6 [`setLanguage`](#set-lang)
  - 3.7 [`setIpAddress`](#set-ip-address)
  - 3.8 [`setUseragent`](#set-user-agent)
  - 3.9 [`setNetworkUserId`](#set-network-user-id)
  - 3.10 [`setDomainUserId`](#set-domain-user-id)
- 4. [Tracking specific events](#events)
  - 4.1 [Common](#common)
    - 4.1.1 [Custom contexts](#custom-contexts)
    - 4.1.2 [Optional timestamp argument](#tstamp-arg)
  - 4.2 [`trackScreenView()`](#screen-view)
  - 4.3 [`trackPageView()`](#page-view)
  - 4.4 [`trackEcommerceTransaction()`](#ecommerce-transaction)
    - 4.4.1 [`TransactionItem`](#ecommerce-transactionitem)
  - 4.5 [`trackStructuredEvent()`](#struct-event)
  - 4.6 [`trackUnstructuredEvent()`](#unstruct-event)
- 5 [Sending event: `Emitter`](#emitters)
  - 5.1 [Using a buffer](#buffer)
  - 5.2 [Choosing the HTTP method](#http-method)
  - 5.3 [Emitter callback](#http-callback)
- 6 [Payload](#payload)
  - 6.1 [Tracker Payload](#tracker-payload)
  - 6.2 [Schema Payload](#schema-payload)
- 7 [Logging](#logging)

<a name="overview" />
## 1. Overview

The [Snowplow Android Tracker](https://github.com/snowplow/snowplow-android-tracker) allows you to track Snowplow events from your Android applications and games. It supports applications using the Android SDK 11 and above.

The tracker should be straightforward to use if you are comfortable with Java development; its API is modelled after Snowplow's [[Python Tracker]] so any prior experience with that tracker is helpful but not necessary. If you haven't already, have a look at the [[Android Tracker Setup]] guide before continuing.

[Back to top](#top)

<a name="init" />
## 2 Initialization

Assuming you have completed the [[Android Tracker Setup]] for your project, you are now ready to initialize the Android Tracker.

<a name="importing" />
#### 2.1 Importing the module

Import the Android Tracker's classes into your Android code like so:

```java
import com.snowplowanalytics.snowplow.tracker.*;
```

That's it - you are now ready to initialize a Tracker instance. 

[Back to top](#top)

<a name="create-tracker" />
#### 2.2 Creating a Tracker

To instantiate a tracker in your code (can be global or local to the process being tracked) simply instantiate the `Tracker` interface with one of the following:

```java
// Create an Emitter
Emitter e1 = new Emitter
        .EmitterBuilder("com.collector.acme", getContext())
        .build();

// Make and return the Tracker object
Tracker t1 = new Tracker
        .TrackerBuilder(e1, "myNamespace", "myAppId")
        .build();
```

This is the most basic Tracker creation possible.  You can expand on this creation with the following builder options:

```java
// Create an Emitter with all options
Emitter e2 = new Emitter
        .EmitterBuilder("com.collector.acme", getContext()) // Required
        .method(HttpMethod.GET) // Optional - Defines how we send the request
        .option(BufferOption.Single) // Optional - Defines how many events we bundle in a POST
        .security(RequestSecurity.HTTPS) // Optional - Defines what protocol used to send events
        .callback(new RequestCallback() { // Optional - Defines custom callback methods
                    @Override
                    public void onSuccess(int successCount) {
                        // Your custom onSuccess callback
                    }
                    @Override
                    public void onFailure(int successCount, int failureCount) {
                       // Your custom onFailure callback
                    }
                })
        .build();
```

```java
// Create a Tracker with all options
Tracker t2 = new Tracker
        .TrackerBuilder(e2, "myNamespace", "myAppId")
        .base64(false) // Optional - Defines if we use base64 encoding
        .platform(DevicePlatforms.Mobile) // Optional - Defines what platform the event will report to be on
        .subject(new Subject()) // Optional - A subject which contains values appended to every event
        .build();
```

As you can see there is a fair amount of modularity to the Trackers creation.

| **Argument Name** | **Description**                              |    **Required?**  | **Default**   |
|------------------:|:---------------------------------------------|:------------------|:--------------|
| `emitter`         | The emitter which sends the events           | Yes               |               |
| `subject`         | The subject that defines a user              | No                | null          |
| `namespace`       | The name of the tracker instance             | Yes               |               |
| `appId`           | The application ID                           | Yes               |               |
| `base64`          | Whether to enable [base 64 encoding][base64] | No                | true          |
| `platform`        | The platform that the Tracker is on          | No                | Mobile        |

<a name="emitter" />
##### 2.3.1 `emitter`

The emitter to which the tracker will send events. See [Emitters](#emitter) for more on emitter configuration.

<a name="subject" />
##### 2.3.2 `subject`

The user which the Tracker will track. This should be an instance of the [Subject](#subject) class. You don't need to set this during Tracker construction; you can use the `Tracker.setSubject` method afterwards. In fact, you don't need to create a subject at all. If you don't, though, your events won't contain user-specific data such as timezone and language.

<a name="namespace" />
##### 2.3.3 `namespace`

If provided, the `namespace` argument will be attached to every event fired by the new tracker. This allows you to later identify which tracker fired which event if you have multiple trackers running.

<a name="app-id" />
##### 2.3.4 `appId`

The `appId` argument lets you set the application ID to any string.

<a name="base64" />
##### 2.3.5 `base64`

By default, unstructured events and custom contexts are encoded into Base64 to ensure that no data is lost or corrupted. You can turn encoding on or off using the Boolean `base64Encoded` argument.

<a name="platform" />
##### 2.3.6 `platform`

The 'platform' allows you to pick from a list of allowed platforms which define what type of device/service the event is being sent from.

[Back to top](#top)

<a name="set-platform" />
#### 2.3.7 Change the tracker's platform with `setPlatform`

You can change the platform by calling:

```java
tracker.setPlatform(DevicePlatforms.'SomeOption');
```

For a full list of supported platforms, please see the [[Snowplow Tracker Protocol]].

[Back to top](#top)

<a name="set-subject" />
#### 2.3.8 Change the tracker's subject with `setSubject`

You can change the subject by creating a new Subject object and then calling:

```java
tracker.setSubject(newSubject);
```

[Back to top](#top)

<a name="set-emitter" />
#### 2.3.9 Change the tracker's emitter with `setEmitter`

You can change the emitter by creating a new Emitter object and then calling:

```java
tracker.setEmitter(newEmitter);
```

[Back to top](#top)

<a name="add-data" />
## 3. Adding extra data: the Subject class

You may have additional information about your application's environment, current user and so on, which you want to send to Snowplow with each event.

The Subject class has a set of `set...()` methods to attach extra data relating to the user to all tracked events:

* [`setUserId`](#set-user-id)
* [`setScreenResolution`](#set-screen-resolution)
* [`setViewport`](#set-viewport)
* [`setColorDepth`](#set-color-depth)
* [`setTimezone`](#set-timezone)
* [`setLanguage`](#set-lang)
* [`setIpAddress`](#set-ip-address)
* [`setUseragent`](#set-user-agent)
* [`setNetworkUserId`](#set-network-user-id)
* [`setDomainUserId`](#set-domain-user-id)

Here are some examples:

```java
s1.setUserID("Kevin Gleason"); 
s1.setLanguage("en-gb");
s1.setScreenResolution(1920, 1080);
```

After that, you can add your Subject to your Tracker like so:
```java
Tracker(emitter, s1, namespace, appId);
// OR
t1.setSubject(s1);
// OR
t1.getSubject().setUserId("Gleason Kevin");
```

[Back to top](#top)

<a name="set-user-id" />
#### 3.1 Set user ID with `setUserId`

You can set the user ID to any string:

```java
s1.setUserId( "{{USER ID}}" )
```

Example:

```java
s1.setUserId("alexd")
```

[Back to top](#top)

<a name="set-screen-resolution" />
#### 3.2 Set screen resolution with `setScreenResolution`

If your Java code has access to the device's screen resolution, then you can pass this in to Snowplow too:

```java
t1.setScreenResolution( {{WIDTH}}, {{HEIGHT}} )
```

Both numbers should be positive integers; note the order is width followed by height. Example:

```java
t1.setScreenResolution(1366, 768)
```

[Back to top](#top)

<a name="set-viewport-dimensions" />
#### 3.3 Set viewport dimensions with `setViewport`

If your Java code has access to the viewport dimensions, then you can pass this in to Snowplow too:

```java
s.setViewport( {{WIDTH}}, {{HEIGHT}} )
```

Both numbers should be positive integers; note the order is width followed by height. Example:

```java
s.setViewport(300, 200)
```

[Back to top](#top)

<a name="set-color-depth" />
#### 3.4 Set color depth with `setColorDepth`

If your Java code has access to the bit depth of the device's color palette for displaying images, then you can pass this in to Snowplow too:

```java
s.setColorDepth( {{BITS PER PIXEL}} )
```

The number should be a positive integer, in bits per pixel. Example:

```java
s.setColorDepth(32)
```

[Back to top](#top)

<a name="set-timezone" />
#### 3.5 Set timezone with `setTimezone`

This method lets you pass a user's timezone in to Snowplow:

```java
s.setTimezone( {{TIMEZONE}} )
```

The timezone should be a string:

```java
s.setTimezone("Europe/London")
```

[Back to top](#top)

<a name="set-lang" />
#### 3.6 Set the language with `setLanguage`

This method lets you pass a user's language in to Snowplow:

```java
s.setLanguage( {{LANGUAGE}} )
```

The language should be a string:

```java
s.setLanguage('en')
```

[Back to top](#top)

<a name="set-ip-address" />
### 3.7 `setIpAddress`

This method lets you pass a user's IP Address in to Snowplow:

```java
s.setIpAddress( {{ip}} );
```

The IP Address should be a string:

```java
s.setIpAddress('127.0.0.1');
```

[Back to top](#top)

<a name="set-user-agent" />
### 3.8 `setUseragent`

This method lets you pass a useragent in to Snowplow:

```java
s.setUseragent( {{useragent}} );
```

The useragent should be a string:

```java
s.setUseragent('Agent Smith');
```

[Back to top](#top)

<a name="set-network-user-id" />
### 3.9 `setNetworkUserId`

This method lets you pass a Network User ID in to Snowplow:

```java
s.setNetworkUserId( {{networkUserId}} );
```
The network user id should be a string:

```java
s.setNetworkUserId("network-id");
```

[Back to top](#top)

<a name="set-domain-user-id" />
### 3.10 `setDomainUserId`

This method lets you pass a Domain User ID in to Snowplow:

```java
s.setDomainUserId( {{domainUserId}} );
```
The domain user id should be a string:

```java
s.setDomainUserId("domain-id");
```

[Back to top](#top)

<a name="events" />
## 4. Tracking specific events

Snowplow has been built to enable you to track a wide range of events that occur when users interact with your websites and apps. We are constantly growing the range of functions available in order to capture that data more richly.

Tracking methods supported by the Java Tracker at a glance:

| **Function**                                                | **Description*                                         |
|------------------------------------------------------------:|:-------------------------------------------------------|
| [`trackScreenView()`](#screen-view)                         | Track the user viewing a screen within the application |
| [`trackPageView()`](#page-view)                             | Track and record views of web pages.                   |
| [`trackEcommerceTransaction()`](#ecommerce-transaction)     | Track an ecommerce transaction and its items           |
| [`trackStructuredEvent()`](#struct-event)                   | Track a Snowplow custom structured event               |
| [`trackUnstructuredEvent()`](#unstruct-event)               | Track a Snowplow custom unstructured event             |

[Back to top](#top)

<a name="common" />
#### 4.1 Common

All events are tracked with specific methods on the tracker instance, of the form `trackXXX()`, where `XXX` is the name of the event to track.

[Back to top](#top)

<a name="custom-contexts" />
#### 4.1.1 Custom contexts

In short, custom contexts let you add additional information about the circumstances surrounding an event in the form of a Map object. Each tracking method accepts an additional optional contexts parameter after all the parameters specific to that method:

```java
t1.trackPageView(String pageUrl, String pageTitle, String referrer);
t1.trackPageView(String pageUrl, String pageTitle, String referrer, List<SchemaPayload> context);
t1.trackPageView(String pageUrl, String pageTitle, String referrer, double timestamp);
t1.trackPageView(String pageUrl, String pageTitle, String referrer, List<SchemaPayload> context, double timestamp);
```

The `context` argument should consist of a `List` of `SchemaPayload` representing an array of one or more contexts. The format of each individual context element is the same as for an [unstructured event](#unstruct-event).

If a visitor arrives on a page advertising a movie, the context dictionary might look like this:

```json
{ 
  "schema": "iglu:com.acme_company/movie_poster/jsonschema/2.1.1",
  "data": {
    "movie_name": "Solaris", 
    "poster_country": "JP", 
    "poster_year": "1978"
  }
}
```

Note that even if there is only one custom context attached to the event, it still needs to be placed in an array.

[Back to top](#top)

<a name="tstamp-arg" />
#### 4.1.2 Optional timestamp & context argument

In all the trackers, we offer a way to set the timestamp if you want the event to show as tracked at a specific time. If you don't, we create a timestamp while the event is being tracked.

Here is an example:
```java
t1.trackPageView("www.page.com", "Example Page", "www.referrer.com");
t1.trackPageView("www.page.com", "Example Page", "www.referrer.com", contextArray);
t1.trackPageView("www.page.com", "Example Page", "www.referrer.com", contextArray, 12348567890);
t1.trackPageView("www.page.com", "Example Page", "www.referrer.com", 12348567890);
```

[Back to top](#top)

<a name="screen-view" />
#### 4.2 Track screen views with `trackScreenView()`

Use `trackScreenView()` to track a user viewing a screen (or equivalent) within your app. Arguments are:

| **Argument** | **Description**                     | **Required?** | **Validation**          |
|-------------:|:------------------------------------|:--------------|:------------------------|
| `name`       | Human-readable name for this screen | No            | String                  |
| `id`         | Unique identifier for this screen   | No            | String                  |
| `context`    | Custom context for the event        | No            | List<SchemaPayload>      |
| `timestamp`  | Optional timestamp for the event    | No            | Long                    |

Example:

```java
t1.trackScreenView("HUD > Save Game", "screen23");
t1.trackScreenView("HUD > Save Game", contextList, 123456);
```

[Back to top](#top)

<a name="page-view" />
#### 4.3 Track pageviews with `trackPageView()`

If you are using Java servlet technology or similar to serve webpages to a browser, you can use `trackPageView()` to track a user viewing a page within your app.

Arguments are:

| **Argument** | **Description**                      | **Required?** | **Validation**          |
|-------------:|:-------------------------------------|:--------------|:------------------------|
| `page_url`   | The URL of the page                  | Yes           | String                  |
| `page_title` | The title of the page                | Yes           | String                  |
| `referrer`   | The address which linked to the page | Yes           | String                  |
| `context`    | Custom context for the event         | No            | List<SchemaPayload>      |
| `timestamp`  | Optional timestamp for the event     | No            | Long                    |

Example:

```java
t1.trackPageView("www.example.com", "example", "www.referrer.com", contextList);
t1.trackPageView("www.example.com", "example", "www.referrer.com");
```

[Back to top](#top)

<a name="ecommerce-transaction" />
#### 4.4 Track ecommerce transactions with `trackEcommerceTransaction()`

Use `trackEcommerceTransaction()` to track an ecommerce transaction.

Arguments:

| **Argument**  | **Description**                      | **Required?** | **Validation**           |
|--------------:|:-------------------------------------|:--------------|:-------------------------|
| `order_id`    | ID of the eCommerce transaction      | Yes           | String                   |
| `total_value` | Total transaction value              | Yes           | Double                   |
| `affiliation` | Transaction affiliation              | Yes           | String                   |
| `tax_value`   | Transaction tax value                | Yes           | Double                   |
| `shipping`    | Delivery cost charged                | Yes           | Double                   |
| `city`        | Delivery address city                | Yes           | String                   |
| `state`       | Delivery address state               | Yes           | String                   |
| `country`     | Delivery address country             | Yes           | String                   | 
| `currency`    | Transaction currency                 | Yes           | String                   |
| `items`       | Items in the transaction             | Yes           | List<TransactionItem>    |
| `context`     | Custom context for the event         | No            | Map                      |
| `timestamp`   | Optional timestamp for the event     | No            | Long                     |

The `items` argument is a `List` of individual `TransactionItem` elements representing the items in the e-commerce transaction. Note that `trackEcommerceTransaction` fires multiple events: one transaction event for the transaction as a whole, and one transaction item event for each element of the `items` `List`. Each transaction item event will have the same timestamp, order_id, and currency as the main transaction event.

[Back to top](#top)

<a name="ecommerce-transactionitem" />
#### 4.4.1 Ecommerce TransactionItem with `trackEcommerceTransaction()`

To instantiate a TransactionItem in your code, simply use the following constructor signature:

```java
trackEcommerceTransactionItem(String order_id, String sku, Double price, Integer quantity, String name, String category, String currency, Map context, long transaction_id)
```

These are the fields that can appear as elements in each `TransactionItem` element of the transaction item `List`:

| **Field**  | **Description**                     | **Required?** | **Validation**           |
|-----------:|:------------------------------------|:--------------|:-------------------------|
| `order_id` | Order ID                            | Yes           | String                   |
| `sku`      | Item SKU                            | No            | String                   |
| `price`    | Item price                          | No            | double                   |
| `quantity` | Item quantity                       | No            | int                      |
| `name`     | Item name                           | No            | String                   |
| `category` | Item category                       | No            | String                   |
| `currency` | Item currency                       | No            | String                   |
| `context`  | Item context                        | No            | List<SchemaPayload>       |
| `timestamp`| Optional timestamp for the event    | No            | Long                     |

Example of tracking a transaction containing two items:

```java
// Example to come, in the meantime here is the type signature:
t1.trackEcommerceTransaction(String order_id, Double total_value, String affiliation, Double tax_value,Double shipping, String city, String state, String country, String currency, List<TransactionItem> items, List<SchemaPayload> context);
t1.trackEcommerceTransaction("6a8078be", 300, "my_affiliate", 30, 10, "Boston", "Massachusetts", "USA", "USD", items, context);
```

[Back to top](#top)

<a name="struct-event" />
#### 4.5 Track structured events with `trackStructuredEvent()`

Use `trackStructuredEvent()` to track a custom event happening in your app which fits the Google Analytics-style structure of having up to five fields (with only the first two required):

| **Argument** | **Description**                                                  | **Required?** | **Validation**    |
|-------------:|:---------------------------------------------------------------  |:--------------|:------------------|
| `category`   | The grouping of structured events which this `action` belongs to | Yes           | String            |
| `action`     | Defines the type of user interaction which this event involves   | Yes           | String            |
| `label`      | A string to provide additional dimensions to the event data      | Yes           | String            |
| `property`   | A string describing the object or the action performed on it     | Yes           | String            |
| `value`      | A value to provide numerical data about the event                | Yes           | Int               |
| `context`    | Custom context for the event                                     | No            | List<SchemaPayload>|
| `timestamp`  | Optional timestamp for the event                                 | No            | Long              |

Example:

```java
t1.trackStructuredEvent("shop", "add-to-basket", "Add To Basket", "pcs", 2);
t1.trackStructuredEvent("shop", "add-to-basket", "Add To Basket", "pcs", 2, 123456.7);
```

[Back to top](#top)

<a name="unstruct-event" />
#### 4.6 Track unstructured events with `trackUnstructuredEvent()`

Custom unstructured events are a flexible tool that enable Snowplow users to define their own event types and send them into Snowplow.

When a user sends in a custom unstructured event, they do so as a JSON of name-value properties, that conforms to a JSON schema defined for the event earlier.

Use `trackUnstructuredEvent()` to track a custom event which consists of a name and an unstructured set of properties. This is useful when:

* You want to track event types which are proprietary/specific to your business (i.e. not already part of Snowplow), or
* You want to track events which have unpredictable or frequently changing properties

The arguments are as follows:

| **Argument**   | **Description**                   |  **Required?** |    **Validation**   |
|---------------:|:----------------------------------|:---------------|:--------------------|
| `eventData`    | The properties of the event       | Yes            | SchemaPayload       |
| `context`      | Custom context for the event      | No             | List<SchemaPayload>  |
| `timestamp`    | Optional timestamp for the event  | No             | Long                |

Example:

```java
t1.trackUnstructuredEvent(eventData, contextList);
```

For more on JSON schema, see the [blog post] [self-describing-jsons].

[Back to top](#top)

<a name="emitters" />
## 5. Sending event: `Emitter`

Events are sent using an `Emitter` class. You can initialize a class with a collector endpoint URL with various options to choose how these events should be sent.
Here are the Emitter interfaces that can be used:

```java
Emitter e2 = new Emitter
        .EmitterBuilder("com.collector.acme", Context context) // Required
        .method(HttpMethod.GET) // Optional - Defines how we send the request
        .option(BufferOption.Single) // Optional - Defines how many events we bundle in a POST
        .security(EmitterSecurity.HTTPS) // Optional - Defines what protocol used to send events
        .callback(new EmitterCallback() {...})
        .build();
```

The `Context` is used for caching events in a [SQLite database](http://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html) in order to avoid losing events to network related issues.

| **Argument Name** | **Description**                                                             |    **Required?**  |  **Default**    |
|------------------:|:----------------------------------------------------------------------------|:------------------|:----------------|
| `URI`             | The collector endpoint URI events will be sent to                           | Yes               |                 |
| `context`         | Used to use to open or create an SQLite database                            | Yes               |                 |
| `method`          | The HTTP method events should be sent with                                  | No                | POST            |
| `option`          | The amount of events to send in a POST (GET is always one)                  | No                | 10              |
| `security`        | Whether to use HTTP or HTTPS to send request                                | No                | HTTP            |
| `callback`        | Lets you pass a callback class to handle succes/failure in sending events.  | No                | null            |

[Back to top](#top)

<a name="how-it-works" />
#### 5.1 How the Emitter works?

The Emitter is configured and setup to run as a background process so it never blocks on the Main Thread or on the UI Thread of the device it is on.

The current Emitter flow goes as follows:

1. Emitter is created.
2. Emitter will check if it has access to the internet.
3. If it is it will begin a recurring check for events to send to the configured collector.
   - This is currently set to be every 5 seconds. (# Configurable-1)
4. If there are events in the SQlite database the emitter will grab up to 250 events from the database and begin sending. (# Configurable-2)
5. Once it has finished sending it will again check for events.
6. If there are no events to be sent 5 times in a row, it will shut itself down. (# Configurable-3)
7. On receiving a new event the Emitter checks again if it is online and will then begin sending again.
8. If there are ever any errors in sending; the events will not be deleted from the database and the emitter will then be shutdown.

All configurable options can be found in the 'constants/TrackerConstants.java' class with the following names:

- EMITTER_TICK
- EMITTER_SEND_LIMIT
- EMITTER_EMPTY_EVENTS_LIMIT

<a name="buffer" />
#### 5.2 Using a buffer

A buffer is used to group events together in bulk before sending them. This is especially handy to reduce network usage. By default, the Emitter buffers up to 10 events before sending them. You can change this to send evenets instantly as soon as they are created like so:

```java
e1.setBufferOption(BufferOption.Single); // 1
// OR
e1.setBufferOption(BufferOption.DefaultGroup); // 10
// OR For heavier event sending...
e1.setBufferOption(BufferOption.HeavyGroup); // 25
```
Here are all the posibile options that you can use:

|  **Option**    | **Description**                                    |
|---------------:|:---------------------------------------------------|
| `Single`       | Events are sent individually                       |
| `DefaultGroup` | Sends events in groups of 10 events or less        |
| `HeavyGroup`   | Sends events in groups of 25 events or less        |

Buffer options will only ever influence how POST request are sent however.  All GET requests will be sent individually.

[Back to top](#top)

<a name="http-method" />
####  5.3 Choosing the HTTP method

Snowplow supports receiving events via both GET and POST requests. In a GET request, each event is sent in individual request. With POST requests, events are bundled together in one request.

You can set the HTTP method in the Emitter constructor:

```java
Emitter e2 = new Emitter
        .EmitterBuilder("com.collector.acme", Context context)
        .method(HttpMethod.GET)
        .build();
```

Here are all the posibile options that you can use:

|  **Option**  | **Description**                                    |
|-------------:|:---------------------------------------------------|
| `GET`        | Sends events as GET requests                       |
| `POST`       | Sends events as POST requests                      |

[Back to top](#top)

<a name="http-callback" />
####  5.4 Emitter callback

If an event fails to send because of a network issue, you can choose to handle the failure case with a callback class to react accordingly. The callback class needs to implement the `EmitterCallback` interface in order to do so. Here is a sample bit of code to show how it could work:

```java
RequestCallback callback = new RequestCallback() {
  @Override
  public void onSuccess(int successCount) {
    Log.d("Tracker", "Buffer length for POST/GET:" + successCount);
  }
  @Override
  public void onFailure(int successCount, int failureCount) {
    Log.d("Tracker", "Failures: " + failureCount + "; Successes: " + successCount);
  }
});

Emitter emitter = new Emitter
        .EmitterBuilder("com.collector.acme", Context context)
        .callback(callback)
        .build();
```

[Back to top](#top)

<a name="payload" />
## 6. Payload

The Payload interface is used for implementing a [TrackerPayload](#tracker-payload) and [SchemaPayload](#schema-payload).

[Back to top](#top)

<a name="tracker-payload" />
#### 6.1 Tracker Payload

A TrackerPayload is used internally within the Android Tracker to create the tracking event payloads that are passed to an Emitter to be sent accordingly.

[Back to top](#top)

<a name="schema-payload" />
#### 6.2 Schema Payload

A SchemaPayload is used primarily as a wrapper around a TrackerPayload. After creating a TrackerPayload, you create a SchemaPayload and use `setData` with the Payload, followed by, `setSchema` to set the schema that the payload will be used against.

This is mainly used under the hood, in the Tracker class but is useful to know if you want to create your own Tracker class.

Here's a short example:
```java
// This is our TrackerPayload that we created
TrackerPayload trackerPayload = new TrackerPayload();
trackerPayload.add("key", "value");

// We wrap that payload in a SchemaPayload before sending it.
SchemaPayload schemaPayload = new SchemaPayload();
schemaPayload.setData(trackerPayload);
schemaPayload.setSchema("iglu:com.snowplowanalytics.snowplow/example/jsonschema/1-0-0");
```

[Back to top](#top)

<a name="logging" />
## 7. Logging

Logging in the Tracker is done using our own Logger class: '/utils/Logger.java'. All logging is actioned based on whether or not the 'DEBUG_MODE' constant is set to true in the 'constants/TrackerConstants.java' class.

To turn off Tracker logging simply change this boolean to false.

[Back to top](#top)

[jsonschema]: http://snowplowanalytics.com/blog/2014/05/13/introducing-schemaver-for-semantic-versioning-of-schemas/
[self-describing-jsons]: http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/
[java-0.6]: https://github.com/snowplow/snowplow/wiki/Android-and-Java-Tracker
[java-0.5]: https://github.com/snowplow/snowplow/wiki/Android-v0.1-and-Java-Tracker-v0.5
[base64]: https://en.wikipedia.org/wiki/Base64