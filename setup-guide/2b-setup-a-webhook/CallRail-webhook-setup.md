<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 2b: setup a Webhook](Setting-up-a-webhook) > **[[CallRail-webhook-setup]]**

## Contents

- 1. [Overview](#overview)  
  - 1.1 [Compatibility](#compat)
- 2. [Setup](#setup)
  - 2.1 [CallRail](#setup-callrail)
  - 2.2 [Snowplow Redshift](#setup-redshift)

<a name="overview" />
## 1. Overview

This webhook integration lets you track completed telephone calls logged by [CallRail] [callrail-website].

For the technical implementation, see [[Callrail webhook adapter]].

<a name="compat" />
### 1.1 Compatibility

* [Snowplow 0.9.10] [snowplow-0.9.10]+ (`POST`-capable collectors only)
* [CallRail webhook API] [callrail-webhooks] as of 1 November 2014

<a name="setup" />
## 2. Setup

Integrating CallRail's webhooks into Snowplow is a two-stage process:

1. Configure CallRail to send events to Snowplow
2. (Optional) Create the CallRail event table into Amazon Redshift

<a name="setup-callrail" />
## 2.1 CallRail

Configuration in CallRail is on a per-company basis, therefore you will need to configure the CallRail webhook for each customer that you want to track calls for.

ADD REST OF GUIDE

<a name="setup-redshift" />
## 2.2 Redshift

If you are running the Snowplow batch (Hadoop) flow with Amazon Redshift, then you should deploy the `call_complete` table into your Amazon Redshift.

You can find the table definition here:

* [com_callrail_call_complete_1.sql] [call-complete-sql]

Make sure to deploy this table into the same schema as your `events` table.

That's it - with this table deployed, your CallRail call complete events should automatically flow through into Redshift.

[callrail-website]: http://www.callrail.com/
[callrail-webhooks]: https://support.callrail.com/hc/en-us/articles/201211133-Webhooks
[snowplow-0.9.10]: https://github.com/snowplow/snowplow/releases/tag/0.9.10

[call-complete-sql]: xxx 
