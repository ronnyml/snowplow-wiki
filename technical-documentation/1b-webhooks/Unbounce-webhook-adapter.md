<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Webhooks**](Webhooks) > Unbounce webhook adapter

## Contents

- 1. [Overview](#overview)  
- 2. [Implementation](#implementation)  
  - 2.1 [Event source](#source)  
  - 2.2 [Snowplow adapter](#adapter)  
- 3. [Events](#events)
- 4. [Context](#context)    
- 5. [See also](#see-also)

<a name="overview" />
## 1. Overview

This webhook adapter lets you track a variety of events logged by [Unbounce] [Unbounce-website].

<a name="implementation" />
## 2. Implementation

<a name="source" />
### 2.1 Event source

Details of the Unbounce webhook format as of 23th June 2016.

This adapter assumes that events are sent via a POST request with all information on the querystring with `application/x-www-form-urlencoded` as the content type.

The querystring's name-value pairs must include a schema parameter, which is set to a valid Iglu self-describing schema URI.

<a name="adapter" />
### 2.2 Snowplow adapter

Implementation: [UnbounceAdapter] [Unbounce-adapter]

Unbounce webhook support was implemented in [Snowplow Release 85] [snowplow-release-85].

<a name="events" />
## 3. Events

This webhook adapter supports any event, as long as it is a valid self-describing event.

<a name="context" />
## 4. Context

All resources for this webhook's context:

| **Context**      | **JSON Schema**                                  | **JSON Paths**                                    | **Redshift Table**                                     |
|:---------------|:-------------------------------------------------|:--------------------------------------------------|:-------------------------------------------------------|
| Event Context                   | [event_context 1-0-0] [event_context-json-schema]                 | [event_context_1.json] [event_context-json-paths]                 | [com_unbounce_event_context_1.sql] [event_context-sql]                 |

<a name="see-also" />
## 5. See also

[[Unbounce webhook setup]]

[Unbounce-website]: https://Unbounce.com/
[Unbounce-adapter]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry/UnbounceAdapter.scala
[snowplow-release-85]: https://github.com/snowplow/snowplow/releases/tag/r85

[event_context-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.unbounce/event_context/jsonschema/1-0-0

[event_context-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.unbounce/event_context_1.json

[event_context-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.unbounce/event_context_1.sql
