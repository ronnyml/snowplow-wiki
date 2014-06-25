<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > [Using registry-aware enrichments](Using-registry-aware-enrichments)

  - 1. [Introduction](#introduction)
  - 2. [common](#common)
  - 3. [The enrichments](#enrichments)
    - 3.1 [IpToGeo](#iptogeo)
    - 3.2 [AnonIp](#anonip)

<a name="introduction"/>
## 1. Introduction

Snowplow offers the option to configure certain enrichments. This is done using configuration JSONs. The config.yml file which the EmrEtlRunner requires has an "enrichments" field which should be populated with the filepath of a directory containing your configuration JSONs.

<a name="common"/>
## 2. Common

Each enrichment has a JSON schema against which it is validated. (For more information on how Snowplow uses JSON schemas, see [this blog post][snowplow-schemas].) For each enrichment, the JSON used to configure the enrichment has:
* A `name` field containing the name of the enrichment
* A `vendor` field containing the name of the company which created the enrichment
* An `enabled` field which can be set to `false` to ignore the enrichment
* A `parameters` field, containing the information needed to configure the enrichment

The `enabled` and `parameters` fields are the ones which you may wish to customize.

<a name="enrichments"/>
## 3. The Enrichments

<a name="iptogeo"/>
### 3.1 IpToGeo Enrichment

This enrichment uses a MaxMind database to look up a user's geographic location based on their IP address, and populates the `geo_country`, `geo_region`, `geo_city`, `geo_zipcode`, `geo_latitude`, and `geo_longitude` fields. [This blog post][maxmind-post] has more information.

It's JSON schema can be found [here][ip-to-geo].

An example config JSON:

```json
{
	"schema": "iglu:com.snowplowanalytics.snowplow/ip_to_geo/jsonschema/1-0-0",

	"data": {

		"name": "ip_to_geo",
		"vendor": "com.snowplowanalytics.snowplow",
		"enabled": true,
		"parameters": {
			"maxmindDatabase": "GeoLiteCity.dat",
			"maxmindUri": "http://snowplow-hosted-assets.s3.amazonaws.com/third-party/maxmind"
		}
	}
}
```

The `maxmindDatabase` field contains the name of the database file. It must be either "GeoLiteCity.dat" or "GeoIPCity.dat".

The `maxmindUri` field contains the URI of the bucket in which the database file is found.

So the above example would correspond to the database file hosted at http://snowplow-hosted-assets.s3.amazonaws.com/third-party/maxmind/GeoLiteCity.dat.

<a name="anonip"/>
### 3.2 AnonIp Enrichment

This enrichment lets you anonymize the IP addresses found in the `user_ipaddress` field by replacing a certain number of octets with "X"s. For example, anonymizing one octet would change the address `255.255.255.255` to `255.255.255.XXX`, and anonymizing three octets would change it to `255.XXX.XXX.XXX`.

It's JSON schema can be found [here][anon-ip]

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


[snowplow-schemas]: http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/
[maxmind-post]: snowplowanalytics.com/blog/2013/05/16/snowplow-0.8.4-released-with-maxmind-geoip/
[anon-ip]: https://github.com/snowplow/iglu-central/blob/feature/enrichments/schemas/com.snowplowanalytics.snowplow/anon_ip/jsonschema/1-0-0
[ip-to-geo]: https://github.com/snowplow/iglu-central/blob/feature/enrichments/schemas/com.snowplowanalytics.snowplow/ip_to_geo/jsonschema/1-0-0
