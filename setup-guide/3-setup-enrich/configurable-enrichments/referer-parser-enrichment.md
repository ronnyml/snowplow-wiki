<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > referer-parser enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0][schema]  
Compatibility 0.9.6+  
Data provider [Snowplow referer-parser][referer-parser-repo]  

### Overview

The IP anonymization enrichment uses the [Snowplow referer-parser][referer-parser-repo] to extract attribution data from referer URLs. You can provide a list of internal subdomains which will be treated as "internal" rather than unknown.

### Example

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0",

	"data": {

		"name": "referer_parser",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"internalDomains": [
				"subdomain1.mysite.com",
				"subdomain2.mysite.com"
			]
		}
	}
}
```

In this example, if an event's referer URL is either "subdomain1.mysite.com" or "subdomain2.mysite.com" it will be counted as internal.

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0
[referer-parser-repo]: https://github.com/snowplow/referer-parser
