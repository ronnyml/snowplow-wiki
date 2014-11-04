[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Webhooks**](Webhooks) > AD-X Tracking webhook adapter

## Contents

- 1. [Overview](#overview)  
- 2. [Implementation](#implementation)  
  - 2.1 [Event source](#source)  
  - 2.2 [Snowplow adapter](#adapter)  
- 3. [Events](#events)  
- 4. [See also](#see-also)

<a name="overview" />
## 1. Overview

This webhook adapter lets you track application installation events logged by [AD-X Tracking] [adxtracking-website].

<a name="implementation" />
## 2. Implementation

<a name="source" />
### 2.1 Event source

Details of the AD-X Tracking webhook format as of 1 November 2014.

AD-X Tracking sends events as a `GET` request with all information on the querystring.

<a name="adapter" />
### 2.2 Snowplow adapter

Implementation: [AdxtrackingAdapter] [adxtracking-adapter]

CallRail webhook support was implemented in [Snowplow 0.9.10] [snowplow-0.9.10].

<a name="events" />
## 3. Events

All resources for this webhook's events:

| **Event**      | **JSON Schema**                                  | **JSON Paths**                                    | **Redshift Table**                                     |
|:---------------|:-------------------------------------------------|:--------------------------------------------------|:-------------------------------------------------------|
| App install  | [app_install.json] [app-install-json-schema] | [app_install_1.json] [app-install-json-paths] | [com_adxtracking_app_install.sql] [app-install-sql] |

<a name="see-also" />
## 4. See also

[[AD-X Tracking webhook setup]]

[adxtracking-website]: http://adxtracking.com/

[adxtracking-adapter]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry/AdxtrackingAdapter.scala
[snowplow-0.9.10]: https://github.com/snowplow/snowplow/releases/tag/0.9.10

[app-install-json-schema]: xxx
[app-install-json-paths]: xxx
[app-install-sql]: xxx
