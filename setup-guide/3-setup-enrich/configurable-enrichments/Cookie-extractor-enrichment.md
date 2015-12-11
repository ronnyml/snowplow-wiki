<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > Cookie extractor enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/cookie_extractor_config/jsonschema/1-0-0][schema]  
Compatibility r72+, Scala Stream Collector only  
Data provider None 

### Overview

One powerful attribute of having Snowplow event collection on your own domain (e.g. `events.snowplowanalytics.com`) is the ability to capture first-party cookies set by other services on your domain such as ad servers or CMSes; these cookies are stored as HTTP headers in the Thrift raw event payload by the Scala Stream Collector.

This community-contributed enrichment lets you specify cookies that you want to extract if found; each extracted cookie will end up a single derived context in the JSON Schema [org.ietf/http_cookie/jsonschema/1-0-0] [http-cookie-schema].

### Example

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/cookie_extractor_config/jsonschema/1-0-0",
	"data": {
		"name": "cookie_extractor_config",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"cookies": ["sp"]
		}
	}
}
```

This default configuration is capturing the Scala Stream Collector's own `sp` cookie - in practice you would probably extract other more valuable cookies available on your company domain. 

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/cookie_extractor_config/jsonschema/1-0-0
[http-cookie-schema]: http://iglucentral.com/schemas/org.ietf/http_cookie/jsonschema/1-0-0
