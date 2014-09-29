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
- 4. [The Subject Class](#subject-class)

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

[base64]: https://en.wikipedia.org/wiki/Base64