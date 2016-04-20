[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow-technical-documentation)

The technical documentation reflects the Snowplow architecture, with six loosely-coupled sub-systems connected by five standardised data protocols/formats:

![architecture] [technical-architecture]

## 1A. Trackers

[Trackers overview](trackers)  
[ActionScript3 Tracker](ActionScript3-Tracker)  
[Android Tracker](Android-Tracker)  
[Arduino Tracker](Arduino-Tracker)  
[.NET Tracker](.NET-Tracker)  
[iOS Tracker](iOS-Tracker)  
[Java Tracker](Java-Tracker)   
[JavaScript Tracker](javascript-tracker)  
[[Google AMP Tracker]]  
[Lua Tracker](Lua-Tracker)  
[Node.js Tracker](Node.js-Tracker)  
[PHP Tracker](PHP-Tracker)  
[Python Tracker](Python-Tracker)  
[Pixel Tracker](pixel-tracker)  
[Ruby Tracker](Ruby-Tracker)  
[Scala Tracker](Scala-Tracker)  
[Unity Tracker](Unity-Tracker)  

## 1B. Webhooks

[[Iglu webhook adapter]]  
[[CallRail webhook adapter]]  
[[MailChimp webhook adapter]]  
[[Mandrill webhook adapter]]  
[[Pagerduty webhook adapter]]  
[[Pingdom webhook adapter]]  
[[SendGrid-webhook-adapter]]  
[[Urban-Airship-Connect-webhook-adapter]]  

### A. [Snowplow tracker protocol](snowplow-tracker-protocol)  

## 2. Collectors

[Collectors overview](collectors)  
[Cloudfront collector](cloudfront)  
[Clojure collector (Elastic Beanstalk)](Clojure-collector)  
[Scala Stream collector](Scala-stream-collector)  

### B. [[Collector logging formats]]

## 3. Enrichment

[Overview](Enrichment)   
[EmrEtlRunner](EmrEtlRunner)   
[Scalding-based Enrichment Process](The-Enrichment-Process)   

### C. [Canonical Snowplow event model](canonical-event-model)

## 4. Storage

[Storage Overview](Storage-documentation)  
[Storage in S3](S3-storage)  
[Storage in Redshift](amazon-redshift-storage)  
[Storage in PostgreSQL](postgresql-storage)   
[Storage in Infobright](infobright-storage) (deprecated)  
[The StorageLoader](The-Storage-Loader)  

### D. Snowplow storage formats (to write)

## 5. Data modeling

[Data modeling overview](data-modeling-documentation)

## 6. Analytics

[Analytics overview](analytics-documentation)

[technical-architecture]: https://d3i6fms1cm1j0i.cloudfront.net/github-wiki/images/snowplow-architecture.png
