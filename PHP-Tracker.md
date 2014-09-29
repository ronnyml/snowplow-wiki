<a name="top" />

## Contents

- 1. [Overview](#overview)
- 2. [Installation](#install)  
  - 2.1 [Composer](#composer)
- 3. [Initialize](#init)

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
```PHP
$emitter = new Emitter("collector_uri");
$subject = new Subject();
$tracker = new Tracker($emitter,$subject);
```