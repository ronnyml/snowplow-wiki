<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers) > Java Tracker

This page refers to version 0.1.0 of the Snowplow Java Tracker.

## Contents

- 1. [Overview](#overview)  
- 2. [Initialization](#init)  
  - 2.1 [Importing the module](#importing)
  - 2.2 [Creating a tracker](#create-tracker)
- 3. [Adding extra data](#add-data)
- 4. [Tracking specific events](#events)
  - 4.1 [Common](#common)
    - 4.1.1 [Custom contexts](#custom-contexts)

<a name="overview" />
## 1. Overview

The [Snowplow Java Tracker](https://github.com/snowplow/snowplow-java-tracker) allows you to track Snowplow events from your Java-based desktop and server apps, servlets and games. It supports JDK6+.

The tracker should be straightforward to use if you are comfortable with Java development; its API is modelled after Snowplow's [[Python Tracker]] so any prior experience with that tracker is helpful but not necessary.

[Back to top](#top)

<a name="init" />
## 2 Initialization

Assuming you have completed the [[Java Tracker Setup]] for your Java project, you are now ready to initialize the Java Tracker.

<a name="importing" />
### 2.1 Importing the module

Import the Java Tracker's classes into your Java code like so:

```java
import com.snowplowanalytics.snowplow.tracker.*;
```

That's it - you are now ready to initialize a TrackerC instance. 

[Back to top](#top)

<a name="create-tracker" />
### 2.2 Creating a Tracker

To instantiate a tracker in your code (can be global or local to the process being tracked) simply instantiate the `Tracker` interface with one of the following:

```java
TrackerC(String collector_URI, String namespace)

TrackerC(String collector_uri, String namespace, String app_id, String context_vendor, boolean base64_encode, boolean enable_contracts)
```

For example:

```java
Tracker t1 = new TrackerC("d3rkrsqld9gmqf.cloudfront.net", "Snowplow Java Tracker Test", "testing_app", "com.snowplow", true, true);
```

TODO: clarify each of the arguments.


## Comments

<a name="add-data" />
## 3. Adding extra data: The Subject class

You may have additional information about your application's environment, current user and so on, which you want to send to Snowplow with each event.

The TrackerC class has a set of `set...()` methods to attach extra data relating to the user to all tracked events:

* [`setUserId`](#set-platform)
* [`setPlatform`](#set-user-id)
* [`setScreenResolution`](#set-screen-resolution)
* [`setViewport`](#set-viewport)
* [`setColorDepth`](#set-color-depth)
* [`setTimezone`](#set-timezone)
* [`setLanguage`](#set-lang)

Here are some examples:

```java
t1.setUserID("Kevin Gleason"); 
t1.setLanguage("eng");
t1.setPlatform("cnsl");
t1.setScreenResolution(1260, 1080);
```

Current supported platforms include "pc", "tv", "mob", "cnsl", and "iot".

TODO: break each of these into its own sub-subject, as in the Python Tracker docs.


	String context = "{'Movie':'Shawshank Redemption', 'Time':'142 Minutes' }"
	Map<String,Object> unstruct_info = new LinkedHashMap<String,Object>();
	unstruct_info.put("Gross movie profit", 28341469);
	...



    t1.track();

----

Now, locally in the function you would like to track, place a tracking call to one of the supported methods. Currently implemented are:

    t1.track_page_view(String page_url, String page_title, String referrer, String context)

    t1.track_struct_event(String category, String action, String label, String property, int value, String vendor, String context)

    t1.track_unstruct_event(String eventVendor, String eventName, String dictInfo, String context)

    t1.track_screen_view(String name, String id, String context)

    t1.track_ecommerce_transaction_item(String order_id, String sku, Double price, Integer quantity, String name, String category, String currency, String context, String transaction_id) *

    t1.track_ecommerce_transaction(String order_id, Double total_value, String affiliation, Double tax_value,Double shipping, String city, String state, String country, String currency, List<Map<String, String>> items, String context) *

###### * Tested and succeeded, but not as much as other functions. Potentially contains bugs.

And thats it! When the tracking function is called, all the information gathered will be sent to the collector URI you have set!

Now you're ready to [view the documentation][documentation]!

Dont have the collector configured? [See Snowplow setup] [setup]

#### Don't need to fill in every field?

A few fields are required, like collector_uri and namespace, or page_url or catrgory and action etc. For the most part, if you dont need a field you can use null! The code will convert those values whether they be number or letters to empty strings which in turn will not show up in the final database.

## Comments

- For a full list of available functions, look into the packages interfaces.
- Context is meant to be in JSON String format

    `String i = "{'Movie':'Shawshank Redemption', 'Time':'100 Minutes' }"`


    Instructions to Use:

     Instantiate a com.snowplowanalytics.snowplow.tracker.PayloadMap and a com.snowplowanalytics.snowplow.tracker.Tracker:
      com.snowplowanalytics.snowplow.tracker.PayloadMap pd = new com.snowplowanalytics.snowplow.tracker.PayloadMapC();
      com.snowplowanalytics.snowplow.tracker.Tracker t1 = new com.snowplowanalytics.snowplow.tracker.TrackerC("collector_uri","namespace");

     Configure payload if needed:
      pd.add_json("{'Movie':'Shawshank Redemption', 'Time':'100 Minutes' }");

     Attach the payload to the com.snowplowanalytics.snowplow.tracker.Tracker:
      t1.setPayload(pd);

     Call the track function when configured:
      t1.track()

    A HttpGet request is configured based on the parameters passed in
     and sent to the collector URI to be stored in a database.



    Current Tracking commands:
     t1.track()
       Submits the current payload to CloudFront server

     track_page_view(page_url, page_title, referrer, context)
       All strings, null context or empty string allowed.
       Configures the URI further adding page data to payload

    Notes:
     Dictionary and JSON context values should be in String format or Map<String,Object> e.g. "{'name':'Kevin', ...}"
     I would recommend using one tracker for one tracking instance type.
       This is because only certain fields are refreshed every loop to reduce overhead at high iteration speed.
     Arrays, Dictionaries, JSON contest is all homogeneous, must all be of the String type.

[documentation]: https://gleasonk.github.io/Saggezza/JavaDoc/index.html