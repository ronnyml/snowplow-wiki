[**HOME**](Home) > [**SNOWPLOW GLOSSARY OF TERMS**](Glossary)


----------

[A]() - [B]() - [C](#C) - [D]() - [E](#E) - [F]() - [G]() - [H]() - [I](#I) - [J]() - [K]() - [L]() - [M]() - [N]() - [O]() - [P](#P) - [Q]() - [R]() - [S](#S) - [T]() - [U]() - [V]() - [W]() - [X]() - [Y]() - [Z]()

----------


<a name="C" />
<a name="e-context" />
### Context

A context is the group of entities associated with or describing the setting in which an event has taken place. What makes contexts interesting is that they are common across multiple different event types. Thus, contexts provide a convenient way in Snowplow to schema common entities once, and then use those schemas across all the different events where those entities are relevant.

Read [more][contexts] or go to the [top](#top)

<a name="E" />
<a name="e-event" />
### Event

An event is something that occurred in a particular point in time. Examples of events include:

- Load a web page
- Add an item to basket
- Enter a destination
- Check a balance
- Search for an item
- Share a video

Snowplow is an event analytics platform. Once you have setup one or more Snowplow trackers, every time an event occurs, Snowplow should spot the event, generate a packet of data to describe the event, and send that event into the Snowplow data pipeline.

Read [more][event] or go to the [top](#top)

### Event Dictionary

When we set up Snowplow, we need to make sure that we track all the [events](e-event) that are meaningful to our business, so that the data associated with those events is available in Snowplow for analysis.

When we come to analyse Snowplow data, we need to be able to look at the event data and understand, in an unambiguous way, what that data actually means i.e. what it represents.

An event dictionary is a crucial tool in both cases. It is a document that defines the universe of events that a company is interested in tracking. 

Read [more][event-dictionary] or go to the [top](#top)

<a name="e-enrichment" />
###Enrichment

Data enrichment is sometimes referred to as "dimension widening". We are using 3rd party sources of data to enrich the data we originally collected about the event so that we have more context available for understanding that event, enabling us to perform richer analysis.

Snowplow supports the following enrichments out-of-the-box. We are working on making our enrichment framework pluggable, so that users and partners can extend the list of enrichments performed as part of the data processing pipeline:

1. IP -> Geographic location
2. Referrer query string -> source of traffic
3. User agent string -> classifying devices, operating systems and browsers

Read [more][enrichment] or go to the [top](#top)

<a name="I" />
###Iglu

Snowplow uses Iglu, a schema respository, to store all the schemas associated with the different [events][e-event] and [contexts][e-context] that are captured via Snowplow. When an event or context is sent into Snowplow, it is sent with a reference to the schema for the [event][e-event] or [context][e-context], which points to the location of the schema for the event or context in Iglu. 

Read [more][iglu] or go to the [top](#top)

<a name="P" />
###Pipeline

The Snowplow pipeline is built to enable a very clean separation of the following steps in the data processing flow:

1. Data collection
2. Data enrichment
3. Data modelling
4. Data analysis

Read [more][pipeline] or go to the [top](#top)

###Self-describing JSON

Self-describing JSONs is an individual JSON with its JSON Schema. Itgenerally looks like the one below:

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

Read [more][self-describing-json] or go to the [top](#top)


[contexts]: http://snowplowanalytics.com/documentation/concepts/contexts/
[event]: http://snowplowanalytics.com/documentation/concepts/events/
[event-dictionary]: http://snowplowanalytics.com/documentation/concepts/event-dictionaries-and-schemas/
[enrichment]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline/#data-enrichment
[iglu]: http://snowplowanalytics.com/documentation/concepts/iglu/
[pipeline]: http://snowplowanalytics.com/documentation/concepts/snowplow-data-pipeline/
[self-describing-json]: http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/