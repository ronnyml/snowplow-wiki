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
    - 4.1.2 [Optional timestamp argument](#tstamp-arg)
    - 4.1.3 [Tracker method return values](#return-values)
  - 4.2 [`track_screen_view()`](#screen-view)
  - 4.3 [`track_page_view()`](#page-view)
  - 4.4 [`track_ecommerce_transaction()`](#ecommerce-transaction)
  - 4.5 [`track_struct_event()`](#struct-event)
  - 4.6 [`track_unstruct_event()`](#unstruct-event)

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
## 3. Adding extra data

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

TODO: break each of these into its own sub-section, as in the Python Tracker docs.

<a name="events" />
## 4. Tracking specific events

Snowplow has been built to enable you to track a wide range of events that occur when users interact with your websites and apps. We are constantly growing the range of functions available in order to capture that data more richly.

Tracking methods supported by the Java Tracker at a glance:

| **Function**                                                        | **Description**                                        |
|--------------------------------------------------------------------:|:-------------------------------------------------------|
| [`track_screen_view()`](#screen-view)                               | Track the user viewing a screen within the application |
| [`track_page_view()`](#page-view)                                   | Track and record views of web pages.                   |
| [`track_ecommerce_transaction()`](#ecommerce-transaction)           | Track an ecommerce transaction and its items           |
| [`track_struct_event()`](#struct-event)                             | Track a Snowplow custom structured event               |
| [`track_unstruct_event()`](#unstruct-event)                         | Track a Snowplow custom unstructured event             |

<a name="common" />
### 4.1 Common

All events are tracked with specific methods on the tracker instance, of the form `track_XXX()`, where `XXX` is the name of the event to track.

<a name="custom-contexts" />
### 4.1.1 Custom contexts

In short, custom contexts let you add additional information about the circumstances surrounding an event in the form of a Java String in JSON format. dictionary object. Each tracking method accepts an additional optional contexts parameter after all the parameters specific to that method:

```java
t1.track_page_view(String page_url, String page_title, String referrer, String context)
```

The `context` argument should consist of a `String` containing a JSON array of one or more contexts. The format of each individual context element is the same as for an [unstructured event](#unstruct-event).

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

Providing the contexts as a Java object (i.e. not as a `String` in JSON format) is not yet supported.

<a name="tstamp-arg" />
### 4.1.2 Optional timestamp argument

Not yet implemented.

<a name="return-value" />
### 4.1.3 Tracker method return values

To be confirmed.

<a name="screen-view" />
### 4.2 Track screen views with `track_screen_view()`

**Warning:** this feature is implemented in the Java tracker, but it is **not** currently supported in the Enrichment, Storage or Analytics stages in the Snowplow data pipeline. As a result, if you use this feature, you will log screen views to your collector logs, but these will not be parsed and loaded into e.g. Redshift to analyse. (Adding this capability is coming soon to Snowplow.)

Use `track_screen_view()` to track a user viewing a screen (or equivalent) within your app. Arguments are:

| **Argument** | **Description**                     | **Required?** | **Validation**          |
|-------------:|:------------------------------------|:--------------|:------------------------|
| `name`       | Human-readable name for this screen | TBC           | TBC                     |
| `id`         | Unique identifier for this screen   | TBC           | TBC                     |
| `context`    | Custom context for the event        | TBC           | TBC                     |

Example:

```java
t1.track_screen_view("HUD > Save Game", "screen23", null);
```

[Back to top](#top)

<a name="page-view" />
### 4.3 Track pageviews with `track_page_view()`

If you are using Java servlet technology or similar to serve webpages to a browser, you can use `track_page_view()` to track a user viewing a page within your app.

Arguments are:

| **Argument** | **Description**                     | **Required?** | **Validation**          |
|-------------:|:------------------------------------|:--------------|:------------------------|
| `page_url`   | The URL of the page                 | TBC           | TBC                     |
| `page_title` | The title of the page               | TBC           | TBC                     |
| `referrer`   | The address which linked to the page| TBC           | TBC                     |
| `context`    | Custom context for the event        | TBC           | TBC                     |

Example:

```java
t1.track_page_view("www.example.com", "example", "www.referrer.com", null);
```

[Back to top](#top)

<a name="ecommerce-transaction" />
### 4.4 Track ecommerce transactions with `track_ecommerce_transaction()`

Use `track_ecommerce_transaction()` to track an ecommerce transaction.

Arguments:

| **Argument**  | **Description**                      | **Required?** | **Validation**           |
|--------------:|:-------------------------------------|:--------------|:-------------------------|
| `order_id`    | ID of the eCommerce transaction      | TBC           | Non-empty string         |
| `total_value` | Total transaction value              | TBC           | Int or Float             |
| `affiliation` | Transaction affiliation              | TBC           | String                   |
| `tax_value`   | Transaction tax value                | TBC           | Int or Float             |
| `shipping`    | Delivery cost charged                | TBC           | Int or Float             |
| `city`        | Delivery address city                | TBC           | String                   |
| `state`       | Delivery address state               | TBC           | String                   |
| `country`     | Delivery address country             | TBC           | String                   | 
| `currency`    | Transaction currency                 | TBC           | String                   |
| `items`       | Items in the transaction             | TBC           | List                     |
| `context`     | Custom context for the event         | TBC           | List                     |

The `items` argument is a `List` of individual `Map<String, String>` elements representing the items in the e-commerce transaction. Note that `track_ecommerce_transaction` fires multiple events: one "transaction" event for the transaction as a whole, and one "transaction item" event for each element of the `items` array. Each transaction item event will have the same timestamp, order_id, and currency as the main transaction event.

These are the fields that can appear as elements in each `Map` element of the transaction item `List`:

| **Field**  | **Description**                     | **Required?** | **Validation**           |
|-----------:|:------------------------------------|:--------------|:-------------------------|
| `ti_id`    | Order ID                            | TBC           | TBC                      |
| `sku`      | Item SKU                            | TBC           | TBC                      |
| `price`    | Item price                          | TBC           | TBC                      |
| `quantity` | Item quantity                       | TBC           | TBC                      |
| `name`     | Item name                           | TBC           | TBC                      |
| `category` | Item category                       | TBC           | TBC                      |
| `context`  | Custom context for the event        | TBC           | TBC                      |

Example of tracking a transaction containing two items:

```java
// Example to come, in the meantime here is the type signature:
t1.track_ecommerce_transaction(String order_id, Double total_value, String affiliation, Double tax_value,Double shipping, String city, String state, String country, String currency, List<Map<String, String>> items, String context);
```

[Back to top](#top)

<a name="struct-event" />
### 4.6 Track structured events with `track_struct_event()`

Use `track_struct_event()` to track a custom event happening in your app which fits the Google Analytics-style structure of having up to five fields (with only the first two required):

| **Argument** | **Description**                                                  | **Required?** | **Validation**           |
|-------------:|:---------------------------------------------------------------  |:--------------|:-------------------------|
| `category`   | The grouping of structured events which this `action` belongs to | Yes           | Non-empty string         |
| `action`     | Defines the type of user interaction which this event involves   | Yes           | Non-empty string         |
| `label`      | A string to provide additional dimensions to the event data      | No            | String                   |
| `property`   | A string describing the object or the action performed on it     | No            | String                   |
| `value`      | A value to provide numerical data about the event                | No            | Int or Float             |
| `context`    | Custom context for the event                                     | No            | List                     |
| `tstamp`     | When the structured event occurred                               | No            | Positive integer         |

Example:

```python
t.track_struct_event("shop", "add-to-basket", None, "pcs", 2)
t1.track_struct_event(String category, String action, String label, String property, int value, String vendor, String context);
```

[Back to top](#top)

<a name="unstruct-event" />
### 4.7 Track unstructured events with `track_unstruct_event()`

**Warning:** this feature is implemented in the Python tracker, but it is **not** currently supported in the Enrichment, Storage or Analytics stages in the Snowplow data pipeline. As a result, if you use this feature, you will log unstructured events to your collector logs, but these will not be parsed and loaded into e.g. Redshift to analyse. (Adding this capability is on the roadmap.)

Use `track_unstruct_event()` to track a custom event which consists of a name and an unstructured set of properties. This is useful when:

* You want to track event types which are proprietary/specific to your business (i.e. not already part of Snowplow), or
* You want to track events which have unpredictable or frequently changing properties

The arguments are as follows:

| **Argument**   | **Description**                      | **Required?** | **Validation**          |
|---------------:|:-------------------------------------|:--------------|:------------------------|
| `event_json`   | The properties of the event          | Yes           | Dict                    |
| `context`      | Custom context for the event         | No            | List                    |
| `tstamp`       | When the unstructured event occurred | No            | Positive integer        |

Example:

```python
t1.track_unstruct_event(String eventVendor, String eventName, String dictInfo, String context);
t.track_unstruct_event({
  "schema": "com.example_company/save-game/jsonschema/1.0.2",
  "data": {
    "save_id": "4321",
    "level": 23,
    "difficultyLevel": "HARD",
    "dl_content": true 
  }
})
```

The `event_json` must be a Python dictionary with two fields: `schema` and `data`. `data` is a flat dictionary containing the properties of the unstructured event. `schema` identifies the JSON schema against which `data` should be validated.

For more on JSON schema, see the [blog post] [self-describing-jsons].

# Scratchpad below this line

	String context = "{'Movie':'Shawshank Redemption', 'Time':'142 Minutes' }"
	Map<String,Object> unstruct_info = new LinkedHashMap<String,Object>();
	unstruct_info.put("Gross movie profit", 28341469);
	...

----

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


    Notes:
     Dictionary and JSON context values should be in String format or Map<String,Object> e.g. "{'name':'Kevin', ...}"

[documentation]: https://gleasonk.github.io/Saggezza/JavaDoc/index.html