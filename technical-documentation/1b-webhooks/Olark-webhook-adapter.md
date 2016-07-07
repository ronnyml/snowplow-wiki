<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Webhooks**](Webhooks) > Olark webhook adapter

## Contents

- 1. [Overview](#overview)  
- 2. [Implementation](#implementation)  
  - 2.1 [Event source](#source)  
  - 2.2 [Snowplow adapter](#adapter)  
- 3. [Events](#events)  
- 4. [See also](#see-also)

<a name="overview" />
## 1. Overview

This webhook adapter lets you track a variety of events logged by [Olark] [olark-website].

<a name="implementation" />
## 2. Implementation

<a name="source" />
### 2.1 Event source

Details of the Olark webhook format as of 7 July 2016.

Olark sends multiple events together as a `POST` request with all information in the body, with `application/x-www-form-urlencoded` as the content type.

<a name="adapter" />
### 2.2 Snowplow adapter

Implementation: [OlarkAdapter] [olark-adapter]

Olark webhook support was implemented in [Snowplow Release 85] [snowplow-release-85].

<a name="events" />
## 3. Events

All resources for this webhook's events:

| **Event**      | **JSON Schema**                                  | **JSON Paths**                                    | **Redshift Table**                                     |
|:---------------|:-------------------------------------------------|:--------------------------------------------------|:-------------------------------------------------------|
| Transcript              | [transcript 1-0-0] [transcript-json-schema]         | [transcript_1.json] [transcript-json-paths]                 | [com_olark_transcript_1.sql] [transcript-sql]                | 
| Offline message         | [offline_message 1-0-0] [message-json-schema]       | [offline_message_1.json] [message-json-paths]| [com_olark_offline_message_1.sql] [message-sql]                  

<a name="see-also" />
## 4. See also

[[Mailgun webhook setup]]

[olark-website]: https://www.olark.com/
[olark-adapter]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry/OlarkAdapter.scala
[snowplow-release-85]: https://github.com/snowplow/snowplow/releases/tag/r85

[transcript-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.olark/transcript/jsonschema/1-0-0
[transcript-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.olark/transcript_1.json
[transcript-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.olark/transcript_1.sql

[message-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.olark/offline_message/jsonschema/1-0-0
[message-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.olark/offline_message_1.json
[message-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.olark/offline_message_1.sql
