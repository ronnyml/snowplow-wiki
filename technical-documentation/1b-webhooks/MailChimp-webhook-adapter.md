<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Webhooks**](Webhooks) > Mailchimp webhook adapter

## Contents

- 1. [Overview](#overview)  
- 2. [Implementation](#implementation)  
  - 2.1 [Event source](#source)  
  - 2.2 [MailChimp adapter](#adapter)  
- 3. [Events](#events)  
- 4. [See also](#see-also)

<a name="overview" />
## 1. Overview

This webhook adapter lets you track a variety of events logged by [MailChimp] [mailchimp-website].

<a name="implementation" />
## 2. Implementation

<a name="source" />
### 2.1 Event source

Details of the MailChimp webhook format as of 1 November 2014.

MailChimp sends events as a `POST` request with all information in the body, formatted as a HTTP Query String.

<a name="adapter" />
### 2.2 MailChimp adapter

Implementation: [MailChimpAdapter] [mailchimp-adapter]

MailChimp webhook support was implemented in [Snowplow 0.9.10] [snowplow-0.9.10].

<a name="events" />
## 3. Events

All resources for this webhook's events:

| **Event**      | **JSON Schema**                                  | **JSON Paths**                                    | **Redshift Table**                                     |
|:---------------|:-------------------------------------------------|:--------------------------------------------------|:-------------------------------------------------------|
| Subscribe               | [subscribe.json] [subscribe-json-schema]               | [subscribe_1.json] [subscribe-json-paths]               | [com_mailchimp_subscribe.sql] [subscribe-sql]               |
| Unsubscribe             | [unsubscribe.json] [unsubscribe-json-schema]           | [unsubscribe_1.json] [unsubscribe-json-paths]           | [com_mailchimp_unsubscribe.sql] [unsubscribe-sql]           |
| Profile Update          | [profile_update.json] [profile-json-schema]            | [profile_update_1.json] [profile-json-paths]            | [com_mailchimp_profile_update.sql] [profile-sql]            |
| Email Address Change    | [email_address_change.json] [email-change-json-schema] | [email_address_change_1.json] [email-change-json-paths] | [com_mailchimp_email_address_change.sql] [email-change-sql] |
| Cleaned Email           | [cleaned_email.json] [email-clean-json-schema]         | [cleaned_email_1.json] [email-clean-json-paths]         | [com_mailchimp_cleaned_email.sql] [email-clean-sql]         |
| Campaign Sending Status | [campaign_sending_status.json] [campaign-json-schema]  | [campaign_sending_status_1.json] [campaign-json-paths]  | [com_mailchimp_campaign_sending_status.sql] [campaign-sql]  |

<a name="see-also" />
## 4. See also

[[Mailchimp webhook setup]]

[mailchimp-website]: http://mailchimp.com/

[mailchimp-adapter]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry/MailchimpAdapter.scala
[snowplow-0.9.10]: https://github.com/snowplow/snowplow/releases/tag/0.9.10

[subscribe-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailchimp/subscribe/jsonschema/1-0-0
[subscribe-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailchimp/subscribe_1.json
[subscribe-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailchimp/subscribe.sql

[unsubscribe-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailchimp/unsubscribe/jsonschema/1-0-0
[unsubscribe-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailchimp/unsubscribe_1.json
[unsubscribe-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailchimp/unsubscribe.sql

[profile-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailchimp/profile_update/jsonschema/1-0-0
[profile-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailchimp/profile_update_1.json
[profile-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailchimp/profile_update.sql

[email-change-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailchimp/email_address_change/jsonschema/1-0-0
[email-change-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailchimp/email_address_change_1.json
[email-change-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailchimp/email_address_change.sql

[email-clean-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailchimp/cleaned_email/jsonschema/1-0-0
[email-clean-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailchimp/cleaned_email_1.json
[email-clean-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailchimp/cleaned_email.sql

[campaign-json-schema]: https://github.com/snowplow/iglu-central/tree/master/schemas/com.mailchimp/campaign_sending_status/jsonschema/1-0-0
[campaign-json-paths]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/jsonpaths/com.mailchimp/campaign_sending_status_1.json
[campaign-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailchimp/campaign_sending_status.sql
