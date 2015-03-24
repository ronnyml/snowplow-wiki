<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enricments > IP lookups enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/ip_lookups/jsonschema/1-0-0][schema]  
Compatibility 0.9.6+  
Data provider [MaxMind][maxmind]  

### Overview

This enrichment uses MaxMind databases to look up useful data based on a user's IP address.

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
| `"geo"`          | [GeoIPCity][geoipcity] or [GeoLiteCity][geolitecity] | Information related to geographic location         | `"GeoLiteCity.dat"` or `"GeoIPCity.dat"`           | `geo_country`, `geo_region`, `geo_city`, `geo_zipcode`, `geo_latitude`, `geo_longitude`, and `geo_region_name` |
| `"isp"`          | [GeoIP ISP][geoipisp]                   | Internet Service Provider                          | `"GeoIPISP.dat"`                                  | `ip_isp`          |
| `"organization"` | [GeoIP Organization][geoiporg]          | Organization name for larger networks              | `"GeoIPOrg.dat"`                                  | `ip_organization` |
| `"domain"`       | [GeoIP Domain][geoipdomain]                | Second level domain name associated with IP address | `"GeoIPDomain.dat"`                               | `ip_domain`       |
| `"netspeed"`     | [GeoIP Netspeed][geoipnetspeed]              | Estimated connection speed                         | `"GeoIPNetSpeed.dat"` or `"GeoIPNetSpeedCell.dat"` | `ip_netspeed`     |

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

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/ip_lookups/jsonschema/1-0-0
[maxmind]: https://www.maxmind.com/en/home
[geoipcity]: https://www.maxmind.com/en/geoip2-city?rld=snowplow
[geolitecity]: http://dev.maxmind.com/geoip/legacy/geolite/?rld=snowplow
[geoipisp]: https://www.maxmind.com/en/isp?rld=snowplow
[geoiporg]: https://www.maxmind.com/en/organization?rld=snowplow
[geoipdomain]: https://www.maxmind.com/en/domain?rld=snowplow
[geoipnetspeed]: https://www.maxmind.com/en/netspeed?rld=snowplow
