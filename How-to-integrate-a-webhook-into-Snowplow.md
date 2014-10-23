There are a few different phases:

### Exploration

* Sign up for the service whose webhooks you will be integrating
* Browse to the service's admin UI
* Switch on webhooks (sometimes called a Streaming or HTTP Response API)
* Provide a Snowplow collector as the target for the webhooks. If you work for Snowplow, you can use http://trial-collector.snplow.com/ For more real-time testing, setup a local collector and then use Ngrok
* Cause all the various webhooks to be triggered, and review their payload structures

### Tickets

(You can skip this step if you don't work for Snowplow Analytics.)

Create three tickets for each webhook you will be supporting:

1. One ticket for the new JSON Schema in `snowplow/iglu-central`
2. One for the new Redshift DDL in `snowplow/snowplow`
3. One for the new JSON Paths file in `snowplow/snowplow`

Also create a ticket in `snowplow/snowplow` called "Scala Common Enrich: add Adapter to pre-process <<Service>> events".

### Schema work

* Create a branch in `snowplow/iglu-central` called `feature/webhooks-<<service>>`
* Create a branch in `snowplow/snowplow` called `feature/webhooks-<<service>>`
* In the branch in `snowplow/iglu-central`, add JSON Schemas for each webhook event
* In the branch in `snowplow/snowplow`, add Redshift DDL for each webhook event
* In the branch in `snowplow/snowplow`, add JSON Path files for each webhook event

### Integration work

* In the branch in `snowplow/snowplow`, add a new file into:

`3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry`

* Implement the pre-processing for this webhook in this file
* Now update the AdapterRegistry to include the new Adapter:

`3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters AdapterRegistry.scala`