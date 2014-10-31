<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 2b: setup a Webhook](Setting-up-a-webhook)

Snowplow allows you to collect events from supported third-party webhooks. These third-party applications (typically Software-as-a-Service) are then responsible for sending their own event streams to [Snowplow Collectors](Setting-up-a-Collector) for further processing.

1. [Choose and configure a Webhook](#choose-configure)

<a name="choose-configure" />
## 1. Choose and configure a Webhook

The following Webhooks are currently available for setup:

| **Webhook**                                | **Description**                                                                          | **Support in Snowplow**              |
|:-------------------------------------------|:-----------------------------------------------------------------------------------------|:-------------------------------------|
| [AD-X Tracking](adxtracking-webhook-setup) | For tracking application and game installs via [AD-X Tracking] [adxtracking-website]     | [Snowplow 0.9.10] [snowplow-0.9.10]+ |
| [CallRail](callrail-webhook-setup)         | For tracking completed telephone calls logged by [CallRail] [callrail-website]           | [Snowplow 0.9.10] [snowplow-0.9.10]+ |
| [MailChimp](mailchimp-webhook-setup)       | For tracking email and email-related events delivered by [MailChimp] [mailchimp-website] | [Snowplow 0.9.10] [snowplow-0.9.10]+ |

If you are interested in sponsoring a new webhook integration for Snowplow, please [talk to us](Talk-to-us).

Back to [Snowplow setup](Setting-up-Snowplow).

[adxtracking-website]: http://adxtracking.com/	
[callrail-website]: http://www.callrail.com/
[mailchimp-website]: http://mailchimp.com/

[snowplow-0.9.10]: https://github.com/snowplow/snowplow/releases/tag/0.9.10
