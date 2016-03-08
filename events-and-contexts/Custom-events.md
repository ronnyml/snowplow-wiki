[**HOME**](Home) » [**EVENTS AND CONTEXTS**](Events-and-Contexts) » [Events Overview](Events-overview) » Custom events

###Overview

Snowplow supports a large number of events "out of the box" (first class citizens), most of which are fairly standard in a web analytics context. See [Snowplow authored events](Snowplow-authored-events) for more details.

If you wish to track an event that Snowplow does not recognise as a first class citizen (i.e. one of the events listed above), then you can track them using one of the following generic events:

- [Custom structured event](#structured-event)
- [Custom unstructured event](#unstructured-event)

<a name="structured-event" />
###Custom structured event

With a structured event, we follow Google five-variable tracking event structure. When you track a structured event, you get five parameters:

- *Category*: The name for the group of objects you want to track.
- *Action*: A string that is used to define the user in action for the category of object.
- *Label*: An optional string which identifies the specific object being actioned.
- *Property*: An optional string describing the object or the action performed on it.
- *Value*: An optional numeric data to quantify or further describe the user action.

For example, when tracking a custom structured event the specification for the `trackStructEvent` method (Javascript tracker) would follow the pattern:

```
snowplow_name_here('trackStructEvent', 'category','action','label','property','value');
```

See [Snowplow tracker protocol](snowplow-tracker-protocol#39-custom-structured-event-tracking) for the specific parameters to be used with structured events and examples of usage.

<a name="unstructured-event" />
###Custom unstructured event

Custom unstructured events are user events which do not fit one of the existing [Snowplow event types](CSnowplow-authored-events) (page views, ecommerce transactions etc), and do not fit easily into our existing [custom structured event format](#structured-event). A **custom unstructured event** consists of two elements:

- A `name` of the unstructured event, e.g. "Game saved" or "returned-order". This is case-sensitive; spaces etc are allowed
- A set of `name: value` properties (also known as a hash, associative array or dictionary)

You might recognise what we call custom unstructured events from other analytics tools including MixPanel, KISSmetrics and Keen.io, where they are the primary trackable event type.

Custom unstructured events are great for a couple of use cases:

- Where you want to track event types which are proprietary/specific to your business (i.e. not already part of Snowplow)
- Where you want to track events which have unpredictable or frequently changing properties

The set of properties in unstructured events is represented with **self-describing JSON** which can have arbitrarily many fields.

> **Self-describing JSON** is an individual JSON with its JSON schema included as a `value` to `schema` key in this JSON.

For example, to track an unstructured event with Javascript tracker, you make use of the `trackUnstructEvent` method with the pattern shown below:

```
snowplow_name_here('trackUnstructEvent', <<SELF-DESCRIBING EVENT JSON>>);
```

See [Snowplow tracker protocol](snowplow-tracker-protocol#310-custom-unstructured-event-tracking) for the specific parameters to be used with structured events and examples of usage.

###Further reading

To find out more about the concepts mentioned above and ultimately how to set up custom events and send them to Snowplow pipeline, follow the links below.

- [Self-describing JSON](https://github.com/snowplow/iglu/wiki/Self-describing-JSONs)
- [Event dictionary]
- [Iglu]