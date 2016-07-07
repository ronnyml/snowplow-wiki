<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2b: setup a Webhook**](Setting-up-a-webhook) > [[Olark webhook setup]]

## Contents

- 1. [Overview](#overview)  
  - 1.1 [Compatibility](#compat)
- 2. [Setup](#setup)
  - 2.1 [Olark](#setup-olark)
  - 2.2 [Snowplow Redshift](#setup-redshift)

<a name="overview" />
## 1. Overview

This webhook integration lets you track events logged by [Olark] [olark-website].

For the technical implementation, see [[Olark webhook adapter]].

<a name="compat" />
### 1.1 Compatibility

* [Snowplow R85] [snowplow-r85]+ (`POST`-capable collectors only)
* [Olark webhook API] [olark-webhooks]

<a name="setup" />
## 2. Setup

Integrating Olark's webhooks into Snowplow is a two-stage process:

1. Configure Olark to send events to Snowplow
2. (Optional) Create the Olark event context table into Amazon Redshift

<a name="setup-olark" />
## 2.1 Olark
Using a webhook is a simple way to take the content of an Olark chat transcript and process it for use on your own server.

First, create a webhook endpoint. A webhook endpoint is a URL on your server that will receive each transcript when a conversation completes, e.g.

```
http://<collector host>/com.olark/v1?aid=<company code>
```

When you finish a live chat or send an offline message, Olark makes HTTP POST to the endpoint you specified. The POST has a single data field, and containing the JSON-encoded transcript.

The second step it to enter the address to your webhook endpoint. Login to your Olark account and go to the integrations page.

[[/setup-guide/images/webhooks/olark/olark-1.png]]

Then, click the "Send Test" button to ensure your endpoint is configured properly.

<a name="setup-redshift" />
## 2.2 Redshift

If you are running the Snowplow batch (Hadoop) flow with Amazon Redshift, then you should deploy the relevant event tables into your Amazon Redshift.

You can find the table definition here:

* [com_olark_transcript_1.sql] [transcript-sql]
* [com_olark_offline_message_1.sql] [message-sql]

Make sure to deploy this table into the same schema as your `events` table.

That's it - with this table deployed, your Olark events should automatically flow through into Redshift.

[olark-website]: https://www.olark.com/
[Olark-webhooks]: https://www.olark.com/help/webhooks
[snowplow-r85]: https://github.com/snowplow/snowplow/releases/tag/r85
[transcript-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.olark/transcript_1.sql
[message-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.olark/offline_message_1.sql
