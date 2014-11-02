<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 2b: setup a Webhook](Setting-up-a-webhook) > **[[CallRail-webhook-setup]]**

## Contents

- 1. [Overview](#overview)  
  - 1.1 [Compatibility](#compat)
- 2. [Setup](#setup)
  - 2.1 [CallRail UI](#)
  - 2.2 [Snowplow Redshift](#)

<a name="overview" />
## 1. Overview

This webhook integration lets you track completed telephone calls logged by [CallRail] [callrail-website].

For the technical implementation, see [[Callrail webhook adapter]].

<a name="compat" />
### 1.1 Compatibility

* [Snowplow 0.9.10] [snowplow-0.9.10]+ (POST-compatible collectors only)
* [CallRail webhook API] [callrail-webhooks] as of 1 November 2014

<a name="setup" />
## 2. Setup

To add.

[callrail-website]: http://www.callrail.com/
[callrail-webhooks]: https://support.callrail.com/hc/en-us/articles/201211133-Webhooks
[snowplow-0.9.10]: https://github.com/snowplow/snowplow/releases/tag/0.9.10
