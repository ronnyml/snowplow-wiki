<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > Weather enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow.enrichments/weather_enrichment_config/jsonschema/1-0-0][schema]  
Compatibility r74+  
Data provider [OpenWeatherMap][owm]  


### Overview

This enrichment uses OpenWeatherMap service to look up weather conditions in which event has been occuried.

There are five possible fields you can add to the "parameters" section of the enrichment configuration JSON: "apiKey", "cacheSize", "geoPrecision", "apiHost", and "timeout".

* apiKey is your key you need to [obtain] [owm-signup] from OpenWeatherMap. Notice that free key coulnd't be used for weather enrichment, you need to subscribe [paid plan] [owm-price].
* cacheSize is amount of requests underlying client need to store. Usually it's amount requests for your plan, plus 1% for errors
* timeout is a time in seconds after which request should be considered failed. Notice that failed weather enrichment will filter out whole your event, whether this failure be timeout or invalid API key
* apiHost is one several (history.openweathermap.org, api.openweathermap.org, pro.openweathermap.org) API hosts. For most cases history.openweathermap.org should be fine
* geoPrecision is fraction of one to which geo coordinates will be rounded for storing in cache. Less precise value is 1 assume ~60km infelicity in worst cast, most precise value is 10 assume ~6km infelicity.

### Example

Here's pretty common configuration suited for [starter plan] [owm-price]:

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow.enrichments/weather_enrichment_config/jsonschema/1-0-0",

    "data": {
        "enabled": true,
        "vendor": "com.snowplowanalytics.snowplow.enrichments",
        "name": "weather_enrichment_config",
        "parameters": {
            "apiKey": "{{KEY}}",
            "cacheSize": 5100,
            "geoPrecision": 1,
            "apiHost": "history.openweathermap.org",
            "timeout": 5
        }
    }
```


[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow.enrichments/weather_enrichment_config/jsonschema/1-0-0
[owm]: http://openweathermap.org
[owm-price]: http://openweathermap.org/price
[owm-signup]: http://home.openweathermap.org/users/sign_up
