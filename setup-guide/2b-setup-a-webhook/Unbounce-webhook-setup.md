<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2b: setup a Webhook**](Setting-up-a-webhook) > [[Unbounce webhook setup]]

## Contents

- 1. [Overview](#overview)  
  - 1.1 [Compatibility](#compat)
- 2. [Setup](#setup)
  - 2.1 [Unbounce](#setup-unbounce)
  - 2.2 [Snowplow Redshift](#setup-redshift)

<a name="overview" />
## 1. Overview

This webhook integration lets you track events logged by [Unbounce] [unbounce-website].

For the technical implementation, see [[Unbounce webhook adapter]].

<a name="compat" />
### 1.1 Compatibility

* [Snowplow R85] [snowplow-r85]+ (`POST`-capable collectors only)
* [Unbounce webhook API] [unbounce-webhooks] as of 23th June 2016

<a name="setup" />
## 2. Setup

Integrating Unbounce's webhooks into Snowplow is a two-stage process:

1. Configure Unbounce to send events to Snowplow
2. (Optional) Create the Unbounce event context table into Amazon Redshift

<a name="setup-unbounce" />
## 2.1 Unbounce
To connect configure your webhook, go to your "Pages" from your dashboard unbounce account.

[[/setup-guide/images/webhooks/unbounce/unbounce-1.png]]

Then click the Configure WebHook link and set the webhook URL.

[[/setup-guide/images/webhooks/unbounce/unbounce-2.png]]

Every time someone makes a new form submission on your page, Unbounce will send the following data to your selected URL.

- data.json   : The submitted form data in JSON format.
- data.xml    : The submitted form data in XML format.
- page_id     : The identifier Unbounce uses to uniquely identify your page.
- page_name   : The name you gave your page.
- page_url    : The URL of the page that contains your form.
- variant     : This identifies the page variant that the visitor saw when they visited your page.

<a name="setup-redshift" />
## 2.2 Redshift

If you are running the Snowplow batch (Hadoop) flow with Amazon Redshift, then you should deploy the `event_context` table into your Amazon Redshift.

You can find the table definition here:

* [com_unbounce_event_context_1.sql] [event-context-sql]

Make sure to deploy this table into the same schema as your `events` table.

That's it - with this table deployed, your Unbounce events should automatically flow through into Redshift.

[unbounce-website]: https://unbounce.com/
[unbounce-webhooks]: http://documentation.unbounce.com/hc/en-us/articles/203510044-Using-a-Webhook
[snowplow-r85]: https://github.com/snowplow/snowplow/releases/tag/r85
[event-context-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.unbounce/event_context_1.sql 
