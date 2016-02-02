[**HOME**](Home) > **SNOWPLOW GLOSSARY OF TERMS**

<a name="top" />

----------

[A](#A) - [B]() - [C](#C) - [D](#D) - [E](#E) - [F]() - [G]() - [H]() - [I](#I) - [J]() - [K]() - [L]() - [M]() - [N]() - [O]() - [P](#P) - [Q]() - [R]() - [S](#S) - [T](#T) - [U](#U) - [V]() - [W](#W) - [X]() - [Y]() - [Z]()

----------

<a name="A" />
<a name="g-analytics" />
####Analytics

Once we have our data [modeled](#g-data-modeling) in tidy users, sessions, content items tables, we are ready to perform analysis on them.

Most companies that use Snowplow will perform analytics using a number of different types of tools:

1. It is common to implement a Business Intelligence tool on top of Snowplow data to enable users (particularly non-technical users) to slice and dice (pivot) on the data. For many companies, the BI tool will be the primary way that most users interface with Snowplow data.
2. Often a data scientist or data science team will often crunch the underlying event-level data to perform more sophisticated analysis including building predictive models, perform marketing attribution etc. The data scientist(s) will use one or more specialist tools e.g. Python for Data Science or R.

Read [more][analytics] or go to the [top](#top)

<a name="C" />
<a name="g-collector" />
####Collector

A collector receives data in the form of `GET` or `POST` requests from the [trackers](#g-tracker), and write the data to either logs or streams (for further processing).

Read [more](collectors) or go to the [top](#top)

<a name="g-context" />
####Context

A context is the group of *entities* associated with or describing the setting in which an event has taken place. What makes contexts interesting is that they are common across multiple different event types. Thus, contexts provide a convenient way in Snowplow to schema common entities once, and then use those schemas across all the different events where those entities are relevant.

Across all our [trackers](#g-tracker), the approach is the same. Each context is a [self-describing JSON](#g-self-describing-json). We create an array of all the different contexts that we wish to pass into Snowplow, and then we pass those contexts in generally as the final argument on any track method that we call to capture the [event](#e-event). 

Read [more][contexts] or go to the [top](#top)

<a name="D" />
<a name="g-data-analysis" />
####Data analysis

See [**Analytics**](#g-analytics)

<a name="g-data-collection" />
####Data collection

At data collection time, we aim to capture all the data required to accurately represent a particular [event](#g-event) that has just occurred.

At this stage, the data that is collected should describe the events as they have happened, including as much rich information about:

1. The event itself
2. The individual/entity that performed the action - that individual or entity is a "context"
3. Any "objects" of the action - those objects are also "context"
4. The wider context that the event has occurred in

For each of the above we want to collect as much data describing the event and associated contexts as possible.

*Read [more][data-collection] or go to the [top](#top)*

<a name="g-data-enrichment" />
####Data enrichment

*See [**Enrichment**](#g-enrichment)*

<a name="g-data-modeling" />
####Data modeling

The data [collection](#g-data-collection) and [enrichment](#g-enrichment) process generates a data set that is an "event stream": a long list of packets of data, where each packet represents a single event.

Whilst it is possible to do analysis directly on this event stream, it is very common to:

1. Join the event-stream data set with other data sets (e.g. customer data, product data, media data, marketing data, financial data).
2. Aggregate the event-level data into smaller data sets that are easier and faster to run analyses against.
3. Apply "business logic" i.e. definitions to the data as part of that aggregation step.

What tables are produced, and the different fields available in each, varies widely between companies in different sectors, and surprisingly even varies within the same vertical. That is because part of putting together these aggregate tables involves implementing business-specific logic.

We call this process of aggregating a "data modeling". At the end of the data modeling process, a clean set of tables is available to make it easier to perform analysis on the data.

*Read [more][data-modeling] or go to the [top](#top)*

<a name="E" />
<a name="e-event" />
####Event

An event is something that occurred in a particular point in time. Examples of events include:

- Load a web page
- Add an item to basket
- Enter a destination
- Check a balance
- Search for an item
- Share a video

Snowplow is an event analytics platform. Once you have setup one or more Snowplow trackers, every time an event occurs, Snowplow should spot the event, generate a packet of data to describe the event, and send that event into the Snowplow data [pipeline](#g-pipeline).

*Read [more][event] or go to the [top](#top)*

####Event Dictionary

When we set up Snowplow, we need to make sure that we track all the [events](#e-event) that are meaningful to our business, so that the data associated with those events is available in Snowplow for analysis.

When we come to analyse Snowplow data, we need to be able to look at the event data and understand, in an unambiguous way, what that data actually means i.e. what it represents.

An event dictionary is a crucial tool in both cases. It is a document that defines the universe of events that a company is interested in tracking. 

*Read [more][event-dictionary] or go to the [top](#top)*

<a name="g-enrichment" />
####Enrichment

Data enrichment is sometimes referred to as "dimension widening". We are using 3rd party sources of data to enrich the data we originally collected about the event so that we have more context available for understanding that event, enabling us to perform richer analysis.

Snowplow supports the following enrichments out-of-the-box. We are working on making our enrichment framework pluggable, so that users and partners can extend the list of enrichments performed as part of the data processing pipeline:

1. IP -> Geographic location
2. Referrer query string -> source of traffic
3. User agent string -> classifying devices, operating systems and browsers

*Read [more][enrichment] or go to the [top](#top)*

<a name="I" />
####Iglu

Snowplow uses Iglu, a schema respository, to store all the schemas associated with the different [events](#g-event) and [contexts](#g-context) that are captured via Snowplow. When an event or context is sent into Snowplow, it is sent with a reference to the schema for the [event](#g-event) or [context](#g-context), which points to the location of the schema for the event or context in Iglu. 

*Read [more][iglu] or go to the [top](#top)*

<a name="P" />
<a name="g-pipeline" />
####Pipeline

The Snowplow pipeline is built to enable a very clean separation of the following steps in the *data processing flow*:

1. [Data collection](#g-data-collection)
2. [Data enrichment](#g-enrichment)
3. [Data modelling](#g-data-modeling)
4. [Data analysis](#g-analytics)

*Read [more][pipeline] or go to the [top](#top)*

<a name="g-self-describing-json" />
####Self-describing JSON

Self-describing JSONs is an individual JSON with its JSON Schema. It generally looks like the one below:

```json
{
	"schema": "iglu:com.snowplowanalytics/ad_click/jsonschema/1-0-0",
	"data": {
		"bannerId": "4732ce23d345"
	}
}
```
It differs from standard JSON due to the following important changes :

1. We have added a new top-level field, schema, which contains (in a space-efficient format) all the information required to uniquely identify the associated JSON Schema
2. We have moved the JSON’s original property inside a data field. This sandboxing will prevent any accidental collisions should the JSON already have a schema field

*Read [more][self-describing-json] or go to the [top](#top)*

<a name="g-storage">
####Storage

The [enrichment](#g-enrichment) process takes raw Snowplow [collector](#g-collector) logs, tidies them up, enriches them (e.g. by adding Geo-IP data, and performing referrer parsing) and then writes the output of that process back to S3 as a cleaned up set of Snowplow event files. The data in these files can be analysed directly by any big data tool that runs on EMR.

In addition, Snowplow data from those event files could be copied into Amazon Redshift, where it can be analysed using any tool that talks to PostgreSQL.

There are therefore a number of different potential storage modules that Snowplow users can store their data in.

*Read [more](Storage-documentation) or go to the [top](#top)*

<a name="g-structured-event" />
####Structured event

We follow Google five-variable tracking event structure. When you track a structured event, you get five parameters:

- *Category*: The name for the group of objects you want to track.
- *Action*: A string that is used to define the user in action for the category of object.
- *Label*: An optional string which identifies the specific object being actioned.
- *Property*: An optional string describing the object or the action performed on it.
- *Value*: An optional numeric data to quantify or further describe the user action. 

For example, when tracking a custom structured event the specification for the `trackStructEvent` method (Javascript tracker) would follow the pattern:

```
snowplow_name_here('trackStructEvent', 'category','action','label','property','value');
```

*Read [more](Canonical-event-model#customstruct) or go to the [top](#top)*

<a name="T" />
<a name="g-tracker" />
####Tracker

A tracker is client- or server-side libraries which track customer behaviour by sending Snowplow [events](#g-event) to a [Collector](#g-collector).

*Read [more](trackers) or go to the [top](#top)*

<a name="U" />
<a name="g-unstructured-event" />
####Unstructured event

You may wish to track [events](#g-event) on your website or application which are not directly supported by Snowplow and which [structured event](#g-structured-event) tracking does not adequately capture. Your event may have more than the five fields offered by `trackStructEvent`, or its fields may not fit into the *category-action-label-property-value* model. The solution is Snowplow's custom unstructured events. Unstructured events use [self-describing JSON](#g-self-describing-json) which can have arbitrarily many fields.

For example, to track an unstructured event with Javascript tracker, you make use the `trackUnstructEvent` method with the pattern shown below:

```
snowplow_name_here('trackUnstructEvent', <<SELF-DESCRIBING EVENT JSON>>);

```

*Read [more][unstructured-events] or go to the [top](#top)*

<a name="W" />
<a name="g-webhook" />
####Webhook

Snowplow allows you to collect events via the adapters (webhooks) of supported third-party software.

Webhooks allow this third-party software to send their own internal event streams to Snowplow Collectors for further processing. Webhooks are sometimes referred to as "streaming APIs" or "HTTP response APIs".

*Read [more](Setting-up-a-webhook) or go to the [top](#top)*


[analytics]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline/#data-analysis
[contexts]: http://snowplowanalytics.com/documentation/concepts/contexts
[data-collection]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline/#data-collection
[data-modeling]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline/#data-modeling
[event]: http://snowplowanalytics.com/documentation/concepts/events
[event-dictionary]: http://snowplowanalytics.com/documentation/concepts/event-dictionaries-and-schemas
[enrichment]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline/#data-enrichment
[iglu]: http://snowplowanalytics.com/documentation/concepts/iglu
[pipeline]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline
[self-describing-json]: http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/
[unstructured-events]: http://snowplowanalytics.com/blog/2013/05/14/snowplow-unstructured-events-guide/
