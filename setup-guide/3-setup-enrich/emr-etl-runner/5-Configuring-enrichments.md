<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configuring enrichments

  - 1. [Introduction](#introduction)
  - 2. [Configuration template](#template)
  - 3. [Configuration defaults](#defaults)
  - 4. [Individual enrichments](#enrichments)
    - 4.1 [ip_lookups](#iplookups)
    - 4.2 [anon_ip](#anonip)
    - 4.3 [referer_parser](#refererparser)
    - 4.4 [campaign_attribution](#campaign_attribution)

<a name="introduction"/>
## 1. Introduction

Snowplow offers the option to configure certain enrichments. This is done using configuration JSONs. The config.yml file which the [EmrEtlRunner](1-Installing-EmrEtlRunner) requires has an "enrichments" field which should be populated with the filepath of a directory containing your configuration JSONs.

<a name="template"/>
## 2. Configuration template

Each enrichment has a JSON schema against which it is validated. (For more information on how Snowplow uses JSON schemas, see [this blog post][snowplow-schemas].) The enrichment JSONs follow a common pattern:

```json
{
	"schema": "iglu:((self-describing JSON schema for the enrichment))",

	"data": {

		"name": "enrichment name",
		"vendor": "enrichment vendor",
		"enabled": true / false,
		"parameters": {
			((enrichment-specific settings))
		}
	}
}
```

For each enrichment, the JSON used to configure the enrichment has:
* A `name` field containing the name of the enrichment
* A `vendor` field containing the name of the company which created the enrichment
* An `enabled` field which can be set to `false` to ignore the enrichment
* A `parameters` field, containing the information needed to configure the enrichment

The `enabled` and `parameters` fields are the ones which you may wish to customize.

<a name="defaults"/>
## 3. Configuration defaults

Snowplow ships with a set of sensible default configurations for each enrichment described on this page.

The folder is browsable on GitHub, it is available as [3-enrich/emr-etl-runner/config/enrichments][enrichment-json-examples].

<a name="enrichments"/>
## 4. Individual enrichments

<a name="iplookups"/>
### 4.1 ip_lookups enrichment

This enrichment uses MaxMind databases to look up useful data based on a user's IP address.

Its JSON schema can be found [here][ip-lookups].

There are five possible fields you can add to the "parameters" section of the enrichment configuration JSON: "geo", "isp", "organization", "domain", and "netspeed". Each of these corresponds to looking up information one of five MaxMind databases, and so needs to have two inner fields:

* The `database` field contains the name of the database file.
* The `uri` field contains the URI of the bucket in which the database file is found. Can have either http: (for publically available MaxMind files) or s3: (for commercial MaxMind files) as the scheme. Must *not* end with a trailing slash.

Here is a maximalist example configuration JSON, which performs all five types of lookup using the MaxMind commercial files:

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/ip_lookups/jsonschema/1-0-0",

	"data": {

		"name": "ip_lookups",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"geo": {
				"database": "GeoIPCity.dat",
				"uri": "s3://my-private-bucket.s3.amazonaws.com/third-party/maxmind"
			},
			"isp": {
				"database": "GeoIPISP.dat",
				"uri": "s3://my-private-bucket.s3.amazonaws.com/third-party/maxmind"				
			},
			"organization": {
				"database": "GeoIPOrg.dat",
				"uri": "s3://my-private-bucket.s3.amazonaws.com/third-party/maxmind"
			},
			"domain": {
				"database": "GeoIPDomain.dat",
				"uri": "s3://my-private-bucket.s3.amazonaws.com/third-party/maxmind"
			},
			"netspeed": {
				"database": "GeoIPNetSpeedCell.dat",
				"uri": "s3://my-private-bucket.s3.amazonaws.com/third-party/maxmind"
			}
		}
	}
}
```

This table contains describes the five types of lookup:

| **Field name**   | **MaxMind Database name**     | **Lookup description**                             | **Accepted database filenames**                   | **Fields populated** |
|-----------------:|:------------------------------|:---------------------------------------------------|:--------------------------------------------------|:---------------------|
| `"geo"`          | [GeoIPCity][geolitecity] or [GeoLiteCity][geolitecity] | Information related to geographic location         | `"GeoLiteCity.dat"` or `"GeoIPCity.dat"           | `geo_country`, `geo_region`, `geo_city`, `geo_zipcode`, `geo_latitude`, `geo_longitude`, and `geo_region_name` |
| `"isp"`          | [GeoIP ISP][geoipisp]                   | Internet Service Provider                          | `"GeoIPISP.dat"`                                  | `ip_isp`          |
| `"organization"` | [GeoIP Organization][geoiporg]          | Organization name for larger networks              | `"GeoIPOrg.dat"`                                  | `ip_organization` |
| `"domain"`       | [GeoIP Domain][geoipdomain]                | Second level domain name associated with IP adress | `"GeoIPDomain.dat"`                               | `ip_domain`       |
| `"netspeed"`     | [GeoIP Netspeed][geoipnetspeed]              | Estimated connection speed                         | `"GeoIPNetSpeed.dat"` or `"GeoIPNetSpeedCell.dat" | `ip_netspeed`     |

**Field name** is the name of the field in the ip_lookups enrichment configuration JSON which you should include if you wish to use that type of lookup. That field should have two subfields: "uri" and "database".

**MaxMind Database name** is the name of the database which that lookup uses.

**Lookup description** describes the lookup.

**Accepted database filenames** are the strings which are allowed in the "database" subfield. If the file name you provide is not one of these, the enrichment JSON will fail validation.

**Fields populated** are the names of the database fields which the lookup fills.

For each of these services you wish to use, add a corresponding field to the enrichment JSON. The fields names you should use are "geo", "isp", "organization", "domain", and "netspeed".

Here is a simpler example configuration (which exactly duplicates the behaviour of Snowplow 0.9.5):

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/ip_lookups/jsonschema/1-0-0",

	"data": {

		"name": "ip_lookups",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"geo": {
				"database": "GeoLiteCity.dat",
				"uri": "http://snowplow-hosted-assets.s3.amazonaws.com/third-party/maxmind"
			}
		}
	}
}
```

This example uses the free GeoLiteCity database hosted by Snowplow.

<a name="anonip"/>
### 4.2 anon_ip enrichment

This enrichment lets you anonymize the IP addresses found in the `user_ipaddress` field by replacing a certain number of octets with "X"s. For example, anonymizing one octet would change the address `255.255.255.255` to `255.255.255.XXX`, and anonymizing three octets would change it to `255.XXX.XXX.XXX`.

Its JSON schema can be found [here][anon-ip]

An example config JSON:

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

<a name="refererparser"/>
### 4.3 referer_parser enrichment

This enrichment uses the [Snowplow Referer-Parser][referer-parser-repo] to extract attribution data from referer URLs. You can provide a list of internal subdomains which will be treated as "internal" rather than unknown.

Its JSON schema can be found [here][referer-parser].

An example config JSON:

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


<a name="campaign_attribution"/>
### 4.4 campaign_attribution enrichment

**This enrichment is not currently supported but will be added in Snowplow version 0.9.7.**

This enrichment lets you choose which querystring parameters will be used to generate the marketing campaign fields `mkt_medium`, `mkt_source`, `mkt_term`, `mkt_content`, and `mkt_campaign`. If you do not enable the campaign_attribution enrichment, those fields will not be populated.

Its JSON schema can be found [here][campaign_attribution].

An example config JSON corresponding to the standard Google parameter names:

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-0",

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
				"mktCampaign": ["utm_campaign"],
			}
		}
	}
}
```

This configuration indicates that, for instance, the `mkt_medium` field in the atomic.events table should be populated by the value of the "utm_medium" field in the querystring.

The Omniture version, in which only the `mkt_campaign` field can be populated:

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-0",

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
				"mktCampaign": ["cid"],
			}
		}
	}
}
```

It is possible to have more than one parameter name in each array, for example:

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-0",

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

The "mapping" field is currently not implemented. In the future, setting it to "script" will indicate that the enrichment uses custom JavaScript to extract the campaign fields from the querystring.

[enrichment-json-examples]: https://github.com/snowplow/snowplow/tree/master/3-enrich/emr-etl-runner/config/enrichments
[snowplow-schemas]: http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/
[maxmind-post]: snowplowanalytics.com/blog/2013/05/16/snowplow-0.8.4-released-with-maxmind-geoip/
[anon-ip]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/anon_ip/jsonschema/1-0-0
[ip-lookups]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/ip_lookups/jsonschema/1-0-0
[referer-parser]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0
[campaign_attribution]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/campaign_attribution/jsonschema/1-0-0
[referer-parser-repo]: https://github.com/snowplow/referer-parser
[geoipcity]: http://dev.maxmind.com/geoip/legacy/install/city/?rld=snowplow
[geolitecity]: http://dev.maxmind.com/geoip/legacy/geolite/?rld=snowplow
[geoipisp]: https://www.maxmind.com/en/isp?rld=snowplow
[geoiporg]: https://www.maxmind.com/en/organization?rld=snowplow
[geoipdomain]: https://www.maxmind.com/en/domain?rld=snowplow
[geoipnetspeed]: https://www.maxmind.com/en/netspeed?rld=snowplow
