<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > Event fingerprint enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/event_fingerprint_config/jsonschema/1-0-0][schema]  
Compatibility Release 71+  
Data provider None

### Overview

This enrichment generates a fingerprint for the event using a hash of client-set fields. This is helpful when deduplicating events.

This is the field which this enrichment will augment:

* event_fingerprint

### Example

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0",

    "data": {

        "name": "event_fingerprint_config",
        "vendor": "com.snowplowanalytics.snowplow",
        "enabled": true,
        "parameters": {
            "excludeParameters": ["eid", "stm"],
            "hashAlgorithm": "MD5"
        }
    }
}
```

The `excludeParameters` field is a list of fields set by the tracker which should be excluded from calculating the hash. In this example, `stm` (which maps to `dvce_sent_tstamp`) is excluded. This is because a tracker may attempt to send the same event twice if it doesn't receive acknowledgement that the first send succeeded. The two copies of the event will have different `stm`s, so this field should not be used to deduplicate. Similiarly, we exclude `eid` (which maps to `event_id`) - the event ID field is already used for deduplication, so nothing further is gained by including it in the hash.

The `hashAlgorithm` field determines the algorithm that should be used to calculate the hash. At the moment, only "MD5" is supported.

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/event_fingerprint_config/jsonschema/1-0-0

[enriched-event-pojo]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/outputs/EnrichedEvent.scala

[enrichment-scala]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/enrichments/registry/JavascriptScriptEnrichment.scala

[string-gotcha]: http://nelsonwells.net/2012/02/json-stringify-with-mapped-variables/
[rhino-experiments]: http://snowplowanalytics.com/blog/2013/10/21/scripting-hadoop-part-1-adventures-with-scala-rhino-and-javascript/

[snowplow-tags]: https://github.com/snowplow/snowplow/tags
