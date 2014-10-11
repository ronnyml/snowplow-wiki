There are a few different phases:

### Preparation

* Sign up for the service whose webhooks you will be integrating
* Browse to the service's admin UI
* Switch on webhooks (sometimes called a Streaming or HTTP Response API)
* Provide a Snowplow collector as the target for the webhooks. If you work for Snowplow, you can use http://trial-collector.snplow.com/ For more real-time testing, setup a local collector and then use Ngrok
* Cause all the various webhooks to be triggered, and review their payload structures
* If you work for Snowplow, create three tickets for each webhook you will be supporting: one ticket for the new JSON Schema in snowplow/iglu-central, one for the new Redshift DDL in snowplow/snowplow and one for the new JSON Paths file in snowplow/snowplow. If you don't work for Snowplow, you can skip this step

### Schema work

* Create a branch in snowplow/iglu-central and add JSON Schemas for each event 