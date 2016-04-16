[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Enrichment**](Enrichment) > [[Stream Enrich]]

Stream Enrich is an [Amazon Kinesis] [kinesis] app, written in Scala and using the Kinesis Client Library, which:

1. **Reads** raw Snowplow events off a Kinesis stream populated by the Scala Stream Collector
2. **Validates** each raw event
2. **Enriches** each event (e.g. infers the location of the user from his/her IP address)
3. **Writes** the enriched Snowplow event to another Kinesis stream

It is designed to be used downstream of the [[Scala Stream Collector]].

It also supports reading raw events from `stdin` and writing enriched events to `stdout`, which is useful for debugging.

Stream Enrich utilizes the [scala-common-enrich][common-enrich] Scala project to enrich events and the [SnowplowRawEvent][schema] for reading Thrift-serialized objects collected with the Scala Stream Collector.

The result of the enrichment process is a TSV representation of the event breakdown of which is outlined below.

#####The application (site, game, app etc) this event belongs to, and the tracker platform
<hr style="border: 1px solid #DDD">
- `app_id`: String
- `platform`: String

#####Date/time
<hr style="border: 1px solid #DDD">
- `etl_tstamp`: String
- `collector_tstamp`: String
- `dvce_created_tstamp`: String

#####Transaction (i.e. this logging event)
<hr style="border: 1px solid #DDD">
- `event`: String
- `event_id`: String
- `txn_id`: String

#####Versioning
<hr style="border: 1px solid #DDD">
- `name_tracker`: String
- `v_tracker`: String
- `v_collector`: String
- `v_etl`: String

#####User and visit
<hr style="border: 1px solid #DDD">
- `user_id`: String
- `user_ipaddress`: String
- `user_fingerprint`: String
- `domain_userid`: String
- `domain_sessionidx`: Integer
- `network_userid`: String

#####Location
<hr style="border: 1px solid #DDD">
- `geo_country`: String
- `geo_region`: String
- `geo_city`: String
- `geo_zipcode`: String
- `geo_latitude`: Float
- `geo_longitude`: Float
- `geo_region_name`: String

#####Other IP lookups
<hr style="border: 1px solid #DDD">
- `ip_isp`: String
- `ip_organization`: String
- `ip_domain`: String
- `ip_netspeed`: String

#####Page
<hr style="border: 1px solid #DDD">
- `page_url`: String
- `page_title`: String
- `page_referrer`: String

#####Page URL components
<hr style="border: 1px solid #DDD">
- `page_urlscheme`: String
- `page_urlhost`: String  
- `page_urlport`: Integer
- `page_urlpath`: String
- `page_urlquery`: String
- `page_urlfragment`: String

#####Referrer URL components
<hr style="border: 1px solid #DDD">
- `refr_urlscheme`: String  
- `refr_urlhost`: String  
- `refr_urlport`: Integer
- `refr_urlpath`: String
- `refr_urlquery`: String
- `refr_urlfragment`: String 

#####Referrer details
<hr style="border: 1px solid #DDD">
- `refr_medium`: String
- `refr_source`: String
- `refr_term`: String

#####Marketing
<hr style="border: 1px solid #DDD">
- `mkt_medium`: String
- `mkt_source`: String
- `mkt_term`: String
- `mkt_content`: String
- `mkt_campaign`: String

#####Custom Contexts
<hr style="border: 1px solid #DDD">
- `contexts`: String

#####Structured Event
<hr style="border: 1px solid #DDD">
- `se_category`: String
- `se_action`: String
- `se_label`: String
- `se_property`: String
- `se_value`: String

#####Unstructured Event
<hr style="border: 1px solid #DDD">
- `unstruct_event`: String

#####Ecommerce transaction (from querystring)
<hr style="border: 1px solid #DDD">
- `tr_orderid`: String
- `tr_affiliation`: String
- `tr_total`: String
- `tr_tax`: String
- `tr_shipping`: String
- `tr_city`: String
- `tr_state`: String
- `tr_country`: String

#####Ecommerce transaction item (from querystring)
<hr style="border: 1px solid #DDD">
- `ti_orderid`: String
- `ti_sku`: String
- `ti_name`: String
- `ti_category`: String
- `ti_price`: String
- `ti_quantity`: String

  // Page Pings
<hr style="border: 1px solid #DDD">
- `pp_xoffset_min`: Integer
- `pp_xoffset_max`: Integer
- `pp_yoffset_min`: Integer
- `pp_yoffset_max`: Integer
  
#####User Agent
<hr style="border: 1px solid #DDD">
- `useragent`: String

#####Browser (from user-agent)
<hr style="border: 1px solid #DDD">
- `br_name`: String
- `br_family`: String
- `br_version`: String
- `br_type`: String
- `br_renderengine`: String

#####Browser (from querystring)
<hr style="border: 1px solid #DDD">
- `br_lang`: String
- `br_features_pdf`: Byte_
- `br_features_flash`: Byte
- `br_features_java`: Byte
- `br_features_director`: Byte
- `br_features_quicktime`: Byte
- `br_features_realplayer`: Byte
- `br_features_windowsmedia`: Byte
- `br_features_gears`: Byte
- `br_features_silverlight`: Byte
- `br_cookies`: Byte
- `br_colordepth`: String
- `br_viewwidth`: Integer
- `br_viewheight`: Integer

  // OS (from user-agent)
<hr style="border: 1px solid #DDD">
- `os_name`: String
- `os_family`: String
- `os_manufacturer`: String
- `os_timezone`: String

#####Device/Hardware (from user-agent)
<hr style="border: 1px solid #DDD">
- `dvce_type`: String
- `dvce_ismobile`: Byte

#####Device (from querystring)
<hr style="border: 1px solid #DDD">
- `dvce_screenwidth`: Integer
- `dvce_screenheight`: Integer

#####Document
<hr style="border: 1px solid #DDD">
- `doc_charset`: String 
- `doc_width`: Integer 
- `doc_height`: Integer

#####Currency
<hr style="border: 1px solid #DDD">
- `tr_currency`: String
- `tr_total_base`: String
- `tr_tax_base`: String
- `tr_shipping_base`: String
- `ti_currency`: String
- `ti_price_base`: String
- `base_currency`: String

#####Geolocation
<hr style="border: 1px solid #DDD">
- `geo_timezone`: String

  // Click ID
<hr style="border: 1px solid #DDD">
- `mkt_clickid`: String
- `mkt_network`: String

#####ETL tags
<hr style="border: 1px solid #DDD">
- `etl_tags`: String

  // Time event was sent
<hr style="border: 1px solid #DDD">
- `dvce_sent_tstamp`: String

  // Referer
<hr style="border: 1px solid #DDD">
- `refr_domain_userid`: String
- `refr_dvce_tstamp`: String

#####Derived contexts
<hr style="border: 1px solid #DDD">
- `derived_contexts`: String

#####Session ID
<hr style="border: 1px solid #DDD">
- `domain_sessionid`: String

#####Derived timestamp
<hr style="border: 1px solid #DDD">
- `derived_tstamp`: String

#####Derived event vendor/name/format/version
<hr style="border: 1px solid #DDD">
- `event_vendor`: String
- `event_name`: String
- `event_format`: String
- `event_version`: String

#####Event fingerprint
<hr style="border: 1px solid #DDD">
- `event_fingerprint`: String

#####True timestamp
<hr style="border: 1px solid #DDD">
- `true_tstamp`: String 


# See also:

+ [Setup guide][setup]
+ [Repository][stream-enrich]

[kinesis]: http://aws.amazon.com/kinesis/

[common-enrich]: https://github.com/snowplow/snowplow/tree/master/3-enrich/scala-common-enrich
[schema]: https://github.com/snowplow/snowplow/blob/master/2-collectors/thrift-schemas/snowplow-raw-event/src/main/thrift/snowplow-raw-event.thrift

[setup]: https://github.com/snowplow/snowplow/wiki/setting-up-stream-enrich
[stream-enrich]: https://github.com/snowplow/snowplow/tree/master/3-enrich/stream-enrich
