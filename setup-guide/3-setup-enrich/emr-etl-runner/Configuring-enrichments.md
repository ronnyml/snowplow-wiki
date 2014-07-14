<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configuring enrichments

  - 1. [Introduction](#introduction)
  - 2. [Configuration template](#template)
  - 3. [Configuration defaults](#defaults)
  - 4. [Individual enrichments](#enrichments)
    - 4.1 [IpLookups](#iplookups)
    - 4.2 [AnonIp](#anonip)
    - 4.3 [RefererParser](#refererparser)

**Warning: This page is not to be used until the release of Snowplow version 0.9.6**

<a name="introduction"/>
## 1. Introduction

Snowplow offers the option to configure certain enrichments. This is done using configuration JSONs. The config.yml file which the [EmrEtlRunner](1-Installing-EmrEtlRunner) requires has an "enrichments" field which should be populated with the filepath of a directory containing your configuration JSONs.

<a name="template"/>
## 2. Configuration template

Each enrichment has a JSON schema against which it is validated. (For more information on how Snowplow uses JSON schemas, see [this blog post][snowplow-schemas].) The enrichment JSONs follow a common pattern:

```json
{
	"schema": "iglu:self-describing JSON schema for the enrichment",

	"data": {

		"name": "enrichment name",
		"vendor": "enrichment vendor",
		"enabled": true / false,
		"parameters": {
			...
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
### 4.1 IpLookups Enrichment

This enrichment uses MaxMind databases to look up useful data based on a user's IP address.

Its JSON schema can be found [here][ip-lookups].

The supported databases are as follows:

1) [GeoIPCity][geolitecity] and the free version [GeoLiteCity][geolitecity] look up a user's geographic location. The enrichment uses this information to populate the `geo_country`, `geo_region`, `geo_city`, `geo_zipcode`, `geo_latitude`, `geo_longitude`, and `geo_region_name` fields. [This blog post][maxmind-post] has more information.

2) [GeoIP ISP][geoipisp] looks up a user's ISP address. This populates the `ip_isp` field.

3) [GeoIP Organization][geoiporg] looks up a user's organization. This populates the `ip_organization` field.

3) [GeoIP Domain][geoipdomain] looks up the second level domain name associated with a user's IP address. This populates the `ip_domain` field.

5) [GeoIP Netspeed][geoipnetspeed] estimates a user's connection speed. This populates the `ip_organization` field.

For each of these services you wish to use, add a corresponding field to the enrichment JSON. The fields names you should use are "geo", "isp", "organization", "domain", and "netspeed".

An example config JSON:

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
			},
			"organization": {
				"database": "GeoIPOrg.dat",
				"uri": "http://my-bucket.s3.amazonaws.com/third-party/maxmind"
			},
		}
	}
}
```

In this example, only the geographic location and organization lookups are performed.

The `database` field contains the name of the database file.

The `uri` field contains the URI of the bucket in which the database file is found.

So the above example would correspond to using the database files hosted at http://snowplow-hosted-assets.s3.amazonaws.com/third-party/maxmind/GeoLiteCity.dat and http://my-bucket.s3.amazonaws.com/third-party/maxmind/GeoIPOrg.dat.

<a name="anonip"/>
### 4.2 AnonIp Enrichment

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
### 4.2 RefererParser Enrichment

This enrichment lets uses the [Snowplow Referer-Parser][referer-parser-repo] to extract attribution data from referer URLs. You can provide a list of internal subdomains which will be treated as "internal" rather than unknown.

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


[enrichment-json-examples]: https://github.com/snowplow/snowplow/tree/master/3-enrich/emr-etl-runner/config/enrichments
[snowplow-schemas]: http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/
[maxmind-post]: snowplowanalytics.com/blog/2013/05/16/snowplow-0.8.4-released-with-maxmind-geoip/
[anon-ip]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/anon_ip/jsonschema/1-0-0
[ip-lookups]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/ip_lookups/jsonschema/1-0-0
[referer-parser]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/referer_parser/jsonschema/1-0-0
[referer-parser-repo]: https://github.com/snowplow/referer-parser
[geoipcity]: http://dev.maxmind.com/geoip/legacy/install/city/
[geolitecity]: http://dev.maxmind.com/geoip/legacy/geolite/rld=snowplow
[geoipisp]: https://www.maxmind.com/en/isprld=snowplow
[geoiporg]: https://www.maxmind.com/en/organizationrld=snowplow
[geoipdomain]: https://www.maxmind.com/en/domainrld=snowplow
[geoipnetspeed]: https://www.maxmind.com/en/netspeed?rld=snowplow
