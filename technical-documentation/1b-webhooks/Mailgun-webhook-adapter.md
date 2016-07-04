<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Webhooks**](Webhooks) > Mailgun webhook adapter

## Contents

- 1. [Overview](#overview)  
- 2. [Implementation](#implementation)  
  - 2.1 [Event source](#source)  
  - 2.2 [Snowplow adapter](#adapter)  
- 3. [Events](#events)  
- 4. [See also](#see-also)

<a name="overview" />
## 1. Overview

This webhook adapter lets you track a variety of events logged by [Mailgun] [mailgun-website].

<a name="implementation" />
## 2. Implementation

<a name="source" />
### 2.1 Event source

Details of the Mailgun webhook format as of 4 July 2016.

Mailgun sends multiple events together as a `POST` request with all information in the body, with `application/x-www-form-urlencoded` as the content type.

<a name="adapter" />
### 2.2 Snowplow adapter

Implementation: [MailgunAdapter] [mailgun-adapter]

Mailgun webhook support was implemented in [Snowplow Release 85] [snowplow-release-85].

<a name="events" />
## 3. Events

All resources for this webhook's events:

| **Event**      | **JSON Schema**                                  | **JSON Paths**                                    | **Redshift Table**                                     |
|:---------------|:-------------------------------------------------|:--------------------------------------------------|:-------------------------------------------------------|
| Bounced              | [message_bounced 1-0-0] [bounced-json-schema]         | [message_bounced_1.json] [bounced-json-paths]                 | [com_mailgun_message_bounced_1.sql] [bounced-sql]                | 
| Clicked              | [message_clicked 1-0-0] [clicked-json-schema]         | [message_clicked_1.json] [clicked-json-paths]                 | [com_mailgun_message_clicked_1.sql] [clicked-sql]                | 
| Complained           | [message_complained 1-0-0] [complained-json-schema]   | [message_complained_1.json] [complained-json-paths] | [com_mailgun_message_complained_1.sql] [complained-sql]                | 
| Delivered            | [message_delivered 1-0-0] [delivered-json-schema]     | [message_delivered_1.json] [delivered-json-paths]| [com_mailgun_message_delivered_1.sql] [delivered-sql]                | 
| Dropped              | [message_dropped 1-0-0] [dropped-json-schema]         | [message_dropped_1.json] [dropped-json-paths]                 | [com_mailgun_message_dropped_1.sql] [dropped-sql]                | 
| Opened               | [message_opened 1-0-0] [opened-json-schema]           | [message_opened_1.json] [opened-json-paths]                 | [com_mailgun_message_opened_1.sql] [opened-sql]                | 
| Unsubscribed         | [recipient_unsubscribed 1-0-0] [unsubscribed-json-schema] | [recipient_unsubscribed_1.json] [unsubscribed-json-paths]| [com_mailgun_recipient_unsubscribed_1.sql] [unsubscribed-sql]                |                   

<a name="see-also" />
## 4. See also

[[Mailgun webhook setup]]

[mailgun-website]: https://mailgun.net/
[mailgun-adapter]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry/MailgunAdapter.scala
[snowplow-release-85]: https://github.com/snowplow/snowplow/releases/tag/r85

[bounced-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/message_bounced/jsonschema/1-0-0
[bounced-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/message_bounced_1.json
[bounced-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_bounced_1.sql

[clicked-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/message_clicked/jsonschema/1-0-0
[clicked-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/message_clicked_1.json
[clicked-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_clicked_1.sql

[complained-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/message_complained/jsonschema/1-0-0
[complained-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/message_complained_1.json
[complained-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_complained_1.sql

[delivered-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/message_delivered/jsonschema/1-0-0
[delivered-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/message_delivered_1.json
[delivered-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_delivered_1.sql

[dropped-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/message_dropped/jsonschema/1-0-0
[dropped-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/message_dropped_1.json
[dropped-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_dropped_1.sql

[opened-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/message_opened/jsonschema/1-0-0
[opened-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/message_opened_1.json
[opened-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_opened_1.sql

[unsubscribed-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailgun/recipient_unsubscribed/jsonschema/1-0-0
[unsubscribed-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailgun/recipient_unsubscribed_1.json
[unsubscribed-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/recipient_unsubscribed_1.sql
