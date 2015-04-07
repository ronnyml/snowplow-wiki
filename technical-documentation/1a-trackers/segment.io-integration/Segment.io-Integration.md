# Snowplow Segment.io Integration Settings

**Progress on this documentation is waiting on the merging of Snowplow's pull requests into Segment's [integrations](https://github.com/segmentio/integrations/pull/221) and [analytics.js-integrations](https://github.com/segmentio/analytics.js-integrations/pull/431) repositories.**

## Settings relevant to both the client-side and server-side integrations

### collectorUrl
The only required setting. This is the collector to which Snowplow should send events, e.g."d1fc8wv8gaz5ca.cloudfront.net". Note that the protocol (http or https) should not be included. 

### appId

Application ID. Should be a unique identifier for the application. Defaults to nothing, i.e. no application ID is set if none is provided.

### trackerNamespace

Name of the Snowplow tracker instance. This is attached to all events sent by the tracker. When using multiple trackers, it helps determine which one created an event. Defaults to "segmentio".

### unstructuredEvents

Whenever a Snowplow user sends an unstructured event, they must include the full schema for that event. The schema specifies how the event should be validated. We need a way for users of the integration to tell us which events should be interpreted as Snowplow unstructured events and which schemas those events should use.

The unstructuredEvents setting is a mapping from events to JSON schemas. For example: 

| **Event name**    | **Schema name**                                        |
|------------------:|:-------------------------------------------------------|
| Viewed Product    | iglu:com.my_company/viewed_product/jsonschema/1-0-0    |
| Added Product     | iglu:com.my_company/add_to_basket/jsonschema/1-1-0     |
| Favorited Product | iglu:com.my_company/favorite_product/jsonschema/1-0-2  |

Then

```
analytics.track('Viewed Product'), {property1: 'one', property2: 2};
```

would fire a Snowplow unstructured event with schema "iglu:com.my_company/viewed_product/jsonschema/1-0-0".

*Note for Segment.io team:* the UI for this would look something like https://segment.io/docs/integrations/google-analytics/#custom-dimensions.

If an event name does not appear in unstructuredEvents but does have a "category" field then we interpret it as a structured event (since "category" is one of the required fields for Snowplow structured events). We use the SegmentIO event name as the Snowplow structured event's action, and attach label, property, and value fields if they are present. Otherwise we ignore it.

### userTraits

The schema to apply to a user traits context. If this is provided, then a custom context based on `analytics.user.traits()` (client-side) or `track.traits()` (server-side) with this schema will be attached to every event fired.

## Settings relevant to only the client-side integration

### version

Version of the Snowplow library sp.js to load. This is used to create the URL from which sp.js is loaded, and defaults to 2.0.0. The default will change whenever new minor versions or patches are released. The current Segment.io implementation will work with sp.js version 1.0.3 as well.

### pagePings

Whether to enable activity tracking. If this is on, then a page ping event will be sent to Snowplow every ten seconds as long as the user remains active on the page. Defaults to true.

### encodeBase64

Whether to Base64 encode unstructured events and custom context JSONS. Defaults to true.

### trackLinks

Whether to enable link click tracking, which sends an event whenever a link is clicked (provided that that link was present when sp.js was loaded). Defaults to true.

### respectDoNotTrack

Whether to turn off tracking when a user has private browsing enabled. Defaults to false.

### cookieDomain

Choose the domain on which the tracker will set cookies. Useful when tracking users across multiple subdomains.