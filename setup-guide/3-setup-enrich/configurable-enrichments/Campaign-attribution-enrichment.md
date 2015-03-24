<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enricments > Campaign attribution enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-0][schema-1]  
Compatibility 0.9.6+  
JSON Schema   [iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-1][schema-2]  
Compatibility r63+  
Data provider [MaxMind][maxmind]  

### Overview

#### 1-0-0

The original version of the campaign attribution enrichment lets you choose which querystring parameters will be used to generate the marketing campaign fields `mkt_medium`, `mkt_source`, `mkt_term`, `mkt_content`, and `mkt_campaign`. If you do not enable the campaign_attribution enrichment, those fields will not be populated.

##### Examples

An example config JSON corresponding to the standard Google parameter names:

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-1",

	"data": {

		"name": "campaign_attribution",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"mapping": "static",
			"fields": {
				"mktMedium": ["utm_medium"],
				"mktSource": ["utm_source"],
				"mktTerm": ["utm_term"],
				"mktContent": ["utm_content"],
				"mktCampaign": ["utm_campaign"]
			}
		}
	}
}
```

This configuration indicates that, for instance, the `mkt_medium` field in the atomic.events table should be populated by the value of the "utm_medium" field in the querystring.

The Omniture version, in which only the `mkt_campaign` field can be populated:

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-1",

	"data": {

		"name": "campaign_attribution",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"mapping": "static",
			"fields": {
				"mktMedium": [],
				"mktSource": [],
				"mktTerm": [],
				"mktContent": [],
				"mktCampaign": ["cid"]
			}
		}
	}
}
```

It is possible to have more than one parameter name in each array, for example:

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-1",

    "data": {

        "name": "campaign_attribution",
        "vendor": "com.snowplowanalytics.snowplow",
        "enabled": false,
        "parameters": {
            "mapping": "static",
            "fields": {
                "mktMedium": ["utm_medium", "medium"],
                "mktSource": ["utm_source", "source"],
                "mktTerm": ["utm_term", "legacy_term"],
                "mktContent": ["utm_content"],
                "mktCampaign": ["utm_campaign", "cid", "legacy_campaign"]
            }
        }
    }
}
```

If multiple acceptable parameter names for the same field are found in the querystring, the first one listed in the configuration JSON will take precedence. For example, using the above configuration, if the querystring contained:

```
"[...]&legacy_campaign=decoy&utm_campaign=mycampaign&cid=anotherdecoy[...]"
```

then the `mkt_campaign` field would be populated with "my_campaign".

#### 1-0-1

Version 1-0-1 of the enrichment will also search the querystring for a name-value pair based on which it can populate the `mkt_clickid` and `mkt_network` fields, which correspond to the click ID and the network responsible for the click. The enrichment automatically knows about Google (corresponding to the "gclid" querystring parameter), Microsoft ("msclkid"), and DoubleClick ("dclid").

For example, if the querystring contained `...&gclid=abc&...` then the `mkt_clickid` field would be populated with `"abc"` and the `mkt_network` field would be populated with `"Google"`.

##### Example

You can add other networks using the `mktClickId` field like this:

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-1",

    "data": {

        "name": "campaign_attribution",
        "vendor": "com.snowplowanalytics.snowplow",
        "enabled": false,
        "parameters": {
            "mapping": "static",
            "fields": {
                "mktMedium": ["utm_medium", "medium"],
                "mktSource": ["utm_source", "source"],
                "mktTerm": ["utm_term", "legacy_term"],
                "mktContent": ["utm_content"],
                "mktCampaign": ["utm_campaign", "cid", "legacy_campaign"],
                "mktClickId": {
                    "customclid": "MyNetwork"
                }
            }
        }
    }
}
```

Then for a querystring containing `...&customclid=abc&...` the `mkt_clickid` field would be populated with `"abc"` and the `mkt_network` field would be populated with `"MyNetwork"`.

The "mapping" field is currently not implemented. In the future, setting it to "script" will indicate that the enrichment uses custom JavaScript to extract the campaign fields from the querystring.


[schema-1]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0
[schema-2]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0
