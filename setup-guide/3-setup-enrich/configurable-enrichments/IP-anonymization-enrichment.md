<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enricments > IP anonymization enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/anon_ip/jsonschema/1-0-0][schema]  
Compatibility 0.9.6+  

### Overview

The IP anonymization enrichment lets you anonymize the IP addresses found in the `user_ipaddress` field by replacing a certain number of octets with "x"s. For example, anonymizing one octet would change the address `255.255.255.255` to `255.255.255.x`, and anonymizing three octets would change it to `255.x.x.x`.

### Example

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/anon_ip/jsonschema/1-0-0",

	"data": {

		"name": "anon_ip",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"anonOctets": 2
		}
	}
}
```

The `anonOctets` field is set to two, so the last two octets of each IP address will be obscured.

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/anon_ip/jsonschema/1-0-0
