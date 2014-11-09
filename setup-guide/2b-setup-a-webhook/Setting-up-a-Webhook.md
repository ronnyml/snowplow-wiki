<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 2b: setup a Webhook](Setting-up-a-webhook)

Snowplow allows you to collect events via the [webhooks] [webhooks-defn] of supported third-party software.

Webhooks allow this third-party software to send their own internal event streams to [Snowplow Collectors](Setting-up-a-Collector) for further processing. Webhooks are sometimes referred to as "streaming APIs" or "HTTP response APIs".

1. [Choose and configure a Webhook](#choose-configure)

<a name="choose-configure" />
## 1. Choose and configure a Webhook

**If you are interested in sponsoring a new webhook integration for Snowplow, please [talk to us](Talk-to-us).**

The following Webhooks are currently available for setup:

| **Webhook** (click for setup)                  | **Description**                                                                          | **Support in Snowplow**     |
|:-----------------------------------------------|:-----------------------------------------------------------------------------------------|:----------------------------|
| **Iglu](Iglu-webhook-setup)** | For tracking [Iglu] [iglu]-compatible self-describing events | [0.9.10] [snowplow-0.9.10]+ |
| **[CallRail](CallRail-webhook-setup)**         | For tracking completed telephone calls logged by [CallRail] [callrail-website]           | [0.9.10] [snowplow-0.9.10]+ |
| **[MailChimp](MailChimp-webhook-setup)**       | For tracking email and email-related events delivered by [MailChimp] [mailchimp-website] | [0.9.10] [snowplow-0.9.10]+ |

**If you are interested in sponsoring a new webhook integration for Snowplow, please [talk to us](Talk-to-us).**

Back to [Snowplow setup](Setting-up-Snowplow).

[webhooks-defn]: http://en.wikipedia.org/wiki/Webhook

[iglu]: xxx	
[callrail-website]: http://www.callrail.com/
[mailchimp-website]: http://mailchimp.com/

[snowplow-0.9.10]: https://github.com/snowplow/snowplow/releases/tag/0.9.10
