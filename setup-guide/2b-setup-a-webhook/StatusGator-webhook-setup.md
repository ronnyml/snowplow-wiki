<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2b: setup a Webhook**](Setting-up-a-webhook) > [[StatusGator webhook setup]]

## Contents

- 1. [Overview](#overview)  
  - 1.1 [Compatibility](#compat)
- 2. [Setup](#setup)
  - 2.1 [StatusGator](#setup-StatusGator)
  - 2.2 [Snowplow Redshift](#setup-redshift)

<a name="overview" />
## 1. Overview

This webhook integration lets you track events logged by [StatusGator] [statusgator-website].
The current event available is: **status_change**.

For the technical implementation, see [[StatusGator webhook adapter]].

<a name="compat" />
### 1.1 Compatibility

* [Snowplow R85] [snowplow-r85]+ (`POST`-capable collectors only)
* [StatusGator webhook API] [statusgator-webhooks] as of 14th June 2016

<a name="setup" />
## 2. Setup

Integrating StatusGator's webhooks into Snowplow is a two-stage process:

1. Configure StatusGator to send events to Snowplow
2. (Optional) Create the StatusGator event table into Amazon Redshift

<a name="setup-StatusGator" />
## 2.1 StatusGator

Web hooks are on all StatusGator paid plans. To enable, sign in and go to the Notification settings page. Turn on Web Hooks and paste in the URL you wish to notify:

[[/setup-guide/images/webhooks/statusgator/statusgator-1.png]]

* The StatusGator web hook format
Each time a service status changes, the webhook will POST some simple attributes to the URL you specify. Example:
```json
{
	"service_name": "Papertrail",
	"favicon_url": "https://dwxjd9cd6rwno.cloudfront.net/favicons/papertrail.ico",
	"status_page_url": "http://www.papertrailstatus.com",
	"home_page_url": "https://papertrailapp.com",
	"current_status": "warn",
	"last_status": "up",
	"occurred_at": "2015-04-15T16:48:52+00:00"
}
```

<a name="setup-redshift" />
## 2.2 Redshift

If you are running the Snowplow batch (Hadoop) flow with Amazon Redshift, then you should deploy the `status_change` table into your Amazon Redshift.

You can find the table definition here:

* [com_statusgator_status_change_1.sql] [status_change-sql]

Make sure to deploy this table into the same schema as your `events` table.

That's it - with this table deployed, your StatusGator status change events should automatically flow through into Redshift.

[statusgator-website]: https://statusgator.com/
[statusgator-webhooks]: https://blog.statusgator.io/introducing-web-hooks/
[snowplow-r85]: https://github.com/snowplow/snowplow/releases/tag/r85
[status_change-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.statusgator/status_change_1.sql 
