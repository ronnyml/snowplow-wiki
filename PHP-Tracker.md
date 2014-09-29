<a name="top" />

## Contents

- 1. [Overview](#overview)
- 2. [Installation](#install)  
  - 2.1 [Composer](#composer)
- 3. [Initialize](#init)
  - 3.1 [Create a Tracker](#create-tracker)
    - 3.1.1 [`emitters`](#emitters)
    - 3.1.2 [`subject`](#subject)
    - 3.1.3 [`namespace`](#namespace)
    - 3.1.4 [`app_id`](#app-id)
    - 3.1.5 [`encode_base64`](#base64)
- 4. [Subjects](#subject-class)
  - 4.1 [`setPlatform`](#set-platform)
  - 4.2 [`setUserId`](#set-user-id)
  - 4.3 [`setScreenRes`](#set-screen-res)
  - 4.4 [`setViewport`](#set-viewport)
  - 4.5 [`setColorDepth`](#set-color-depth)
  - 4.6 [`setTimezone`](#set-timezone)
  - 4.7 [`setLang`](#set-lang)
- 5. [Emitters](#emitter-class)
- 6. [Tracking an Event](#track-an-event)
  - 6.1 [Optional Tracking Arguments](#tracking-options)
    - 6.1.1 [Custom Context](#custom-context)
    - 6.1.2 [Timestamp](#timestamp)
  - 6.2 [Event Tracking Methods](#tracking-methods)
    - 6.2.1 [`trackPageView`](#page-view)
    - 6.2.2 [`trackEcommerceTransaction`](#ecommerce-transaction)
    - 6.2.3 [`trackScreenView`](#screen-view) 
    - 6.2.4 [`trackStructEvent`](#struct-event)  
    - 6.2.5 [`trackUnstructEvent`](#unstruct-event)

<a name="overview" />
## 1. Overview

The [Snowplow PHP Tracker](https://github.com/snowplow/snowplow-php-tracker) allows you to track Snowplow events from your PHP apps and code.

There are three basic types of object you will create when using the Snowplow PHP Tracker: subjects, emitters, and trackers.

A subject represents a user whose events are tracked. A tracker constructs events and sends them to one or more emitters. Each emitter then sends the event to the endpoint you configure, a Snowplow Collector.

<a name="install" />
## 2. Installation

<a name="composer" />
### 2.1 Composer

Using Composer to manage your dependencies, simply add the Snowplow PHP Tracker to your project by including it in your composer.json file.

```json
{
    "require": {
        "snowplow/tracker": ""
    }
}
```

Assuming you have Composer setup correctly in the root of your project.
Type the following command line argument:
```sh
composer install
composer update
```
 - install (If composer has not been run yet)
 - update (If composer dependencies are already installed)

All the dependencies for the Snowplow Tracker are now ready.

<a name="init" />
## 3. Initialize

To instantiate a new Tracker instance we need to make sure the Snowplow Tracker classes are available.

Include these 'use' lines in your project.
```PHP
use Snowplow\Tracker\Tracker;
use Snowplow\Tracker\Emitter;
use Snowplow\Tracker\Subject;
```

We can now create our Emitter, Subject and Tracker objects.

<a name="create-tracker" />
### 3.1 Creating a Tracker

The most basic Tracker instance will only require you to provide the URI of the collector to which the Tracker will log events.
```PHP
$emitter = new Emitter("collector_uri");
$subject = new Subject();
$tracker = new Tracker($emitter,$subject);
```

Other Tracker arguments:

| **Argument Name** | **Description**                      | **Required?** | **Default**          |
|-------------:|:-------------------------------------|:--------------|:------------------------|
| `emitters`      | The emitter to which events are sent       | Yes           | `None`        |
| `subject`   | The user being tracked               |         Yes            | `Subject()` |
| `namespace`  | The name of the tracker instance     |  No           |  `None` |
| `app_id` | The application ID          | No           | `None`         |
| `encode_base64` | Whether to enable [base 64 encoding] [base64] | No | `True`  |

Another example using all allowed arguments:
```PHP
$tracker = new Tracker($emitter,$subject,"cf","cf29ea","1");
```

<a name="emitters" />
#### 3.1.1 `emitters`

This can be either single emitter or an array of emitters. The tracker will send events to all of these emitters, which will in turn send them on to a collector.

```PHP
$emitter = new Emitter("collector_uri");
$emitters = array($emitter1, $emitter2);
```

<a name="subject" />
#### 3.1.2 `subject`

The user which the Tracker will track. This will give your events user-specific data such as timezone and language. You change the subject of your tracker at any time by calling `updateSubject($new_subject_object)`.
All events sent from this Tracker will now have the new subject information appended.

<a name="namespace" />
#### 3.1.3 `namespace`

If provided, the `namespace` argument will be attached to every event fired by the new tracker. This allows you to later identify which tracker fired which event if you have multiple trackers running.

<a name="app-id" />
#### 3.1.4 `app_id`

The `app_id` argument lets you set the application ID to any string.

<a name="base64" />
#### 3.1.5 `encode_base64`

By default, unstructured events and custom contexts are encoded into Base64 to ensure that no data is lost or corrupted. You can turn encoding on or off using the Boolean `encode_base64` argument.

<a name="subject-class" />
## 4. Subjects

For any additional information about your application's environment, current user and so on, which you want to send to Snowplow with each event we have the subject object.

To create a new subject:

```PHP
use Snowplow\Tracker\Subject;
$subject = new Subject();
```

By default the subject has one pair of information in it already, platform ["plat"].

The Subject class contains a variety of 'set' methods to attach extra data to your event.

* [`setPlatform`](#set-platform)
* [`setUserId`](#set-user-id)
* [`setScreenRes`](#set-screen-res)
* [`setViewport`](#set-viewport)
* [`setColorDepth`](#set-color-depth)
* [`setTimezone`](#set-timezone)
* [`setLang`](#set-lang)

<a name="set-platform" />
### 4.1 `setPlatform`

<a name="set-user-id" />
### 4.2 `setUserId`

<a name="set-screen-res" />
### 4.3 `setScreenRes`

<a name="set-viewport" />
### 4.4 `setViewport`

<a name="set-color-depth" />
### 4.5 `setColorDepth`

<a name="set-timezone" />
### 4.6 `setTimezone`

<a name="set-lang" />
### 4.7 `setLang`

<a name="emitter-class" />
## 5. Emitters

<a name="track-an-event" />
## 6. Tracking an Event

Snowplow has been built to enable you to track a wide range of events that occur when users interact with your websites and apps. We are constantly growing the range of functions available so as to capture that data more richly.

Tracking methods supported by the PHP Tracker:

| **Function**                                  | **Description**                                        |
|----------------------------------------------:|:-------------------------------------------------------|
| [`trackPageView`](#page-view)             | Track and record views of web pages.                   |
| [`trackEcommerceTransaction`](#ecommerce-transaction)   | Track an ecommerce transaction           |
| [`trackScreenView`](#screen-view)         | Track the user viewing a screen within the application |
| [`trackStructEvent`](#struct-event)       | Track a Snowplow custom structured event               |
| [`trackUnstructEvent`](#unstruct-event)   | Track a Snowplow custom unstructured event             |

<a name="tracking-options">
### 6.1 Optional Tracking Arguments

<a name="custom-context" />
#### 6.1.1 Custom Context

Custom contexts let you add additional information about any circumstances surrounding an event in the form of a PHP Array of name-value pairs. Each tracking method accepts an additional optional contexts parameter after all the parameters specific to that method:

```PHP
public function trackPageView($page_url, $page_title = NULL, $referrer = NULL, $context = NULL, $tstamp = NULL)
```

An example of a Context Array Structure:

```PHP
array(
    "schema" => "iglu:com.acme_company/movie_poster/jsonschema/2.1.1",
    "data" => array(
        "movie_name" => "Solaris", 
        "poster_country" => "JP"
    )
)
```

This is how to fire a page view event with the above custom context:

```PHP
$tracker->trackPageView(
    "http://www.films.com", 
    "Homepage", 
    NULL,
    array(
        "schema" => "iglu:com.acme_company/movie_poster/jsonschema/2.1.1",
        "data" => array(
            "movie_name" => "Solaris", 
            "poster_country" => "JP"
        )
    )
);
```

<a name="timestamp" />
#### 6.1.2 Timestamp

Each tracking method supports an optional timestamp as its final argument; this allows you to manually override the timestamp attached to this event. The timestamp should be in <b>milliseconds</b> since the Unix epoch.

If you do not pass this timestamp in as an argument, then the PHP Tracker will use the current time to be the timestamp for the event.

Here is an example tracking a structured event and supplying the optional timestamp argument. We can explicitly supply a `NULL` for the intervening arguments which are empty:

```PHP
$tracker->trackStructEvent("some cat", "save action", NULL, NULL, NULL, 1368725287000);
```

<a name="tracking-methods" />
## 6.2 Event Tracking Methods

<a name="page-view" />
### 6.2.1 `trackPageView`

Track a user viewing a page within your app.

Function:
```PHP
public function trackPageView($page_url, $page_title = NULL, $referrer = NULL, $context = NULL, $tstamp = NULL)
```

Arguments:

| **Argument** | **Description**                     | **Required?** | **Validation**          |
|-------------:|:------------------------------------|:--------------|:------------------------|
| `$page_url`   | The URL of the page                 | Yes           | Non-empty string        |
| `$page_title` | The title of the page               | No            | String                  |
| `$referrer`   | The address which linked to the page| No            | String                  |
| `$context`    | Custom context for the event        | No            | List                    |
| `$tstamp`     | When the pageview occurred          | No            | Positive integer        |


[base64]: https://en.wikipedia.org/wiki/Base64