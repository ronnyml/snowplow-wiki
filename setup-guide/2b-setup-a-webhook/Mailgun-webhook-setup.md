<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2b: setup a Webhook**](Setting-up-a-webhook) > [[Mailgun webhook setup]]

## Contents

- 1. [Overview](#overview)  
  - 1.1 [Compatibility](#compat)
- 2. [Setup](#setup)
  - 2.1 [Mailgun](#setup-mailgun)
  - 2.2 [Snowplow Redshift](#setup-redshift)

<a name="overview" />
## 1. Overview

This webhook integration lets you track a variety of events logged by [Mailgun] [mailgun-website].

Available events are:

- Message bounced
- Message clicked
- Message complained
- Message delivered
- Message dropped
- Message opened
- Recipient unsubscribed

For the technical implementation, see [[Mailgun webhook adapter]].

<a name="compat" />
### 1.1 Compatibility

* [Snowplow R85] [snowplow-r85]+ (`POST`-capable collectors only)
* [Mailgun webhook API] [mailgun-webhooks]

<a name="setup" />
## 2. Setup

Integrating Mailgun's webhooks into Snowplow is a two-stage process:

1. Configure Mailgun to send events to Snowplow
2. (Optional) Create the Mailgun events tables into Amazon Redshift

<a name="setup-mailgun" />
## 2.1 Mailgun

Mailgun can make an HTTP POST to a callback URL when events occur with your messages. Webhooks are at the domain level so you can provide a unique URL for each domain. 

To enable webhooks, login into your mailgun account and go to the Webhooks tab in the Dashboard. Set the right webhook URl for the event you want to be notified. Mailgun recommend using bin.mailgun.net for creating temporary URLs to test and debug your webhooks. 

[[/setup-guide/images/webhooks/mailgun/mailgun-1.png]]

<a name="setup-redshift" />
## 2.2 Redshift

If you are running the Snowplow batch (Hadoop) flow with Amazon Redshift, then you should deploy the relevent event tables into your Amazon Redshift.

You can find the table definitions here:

* [com_mailgun_message_bounced_1.sql] [bounced-sql] 
* [com_mailgun_message_clicked_1.sql] [clicked-sql]
* [com_mailgun_message_complained_1.sql] [complained-sql]
* [com_mailgun_message_delivered_1.sql] [delivered-sql]            
* [com_mailgun_message_dropped_1.sql] [dropped-sql]        
* [com_mailgun_message_opened_1.sql] [opened-sql]        
* [com_mailgun_message_unsubscribed_1.sql] [unsubscribed-sql]    

Make sure to deploy this table into the same schema as your `events` table.

That's it - with this table deployed, your Mailgun events should automatically flow through into Redshift.

[mailgun-website]: https://mailgun.net/
[mailgun-webhooks]: https://documentation.mailgun.com/user_manual.html#webhooks
[snowplow-release-85]: https://github.com/snowplow/snowplow/releases/tag/r85

[bounced-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_bounced_1.sql
[clicked-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_clicked_1.sql
[complained-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_complained_1.sql
[delivered-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_delivered_1.sql
[dropped-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_dropped_1.sql
[opened-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_opened_1.sql
[unsubscribed-sql]: https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql/com.mailgun/message_unsubscribed_1.sql
