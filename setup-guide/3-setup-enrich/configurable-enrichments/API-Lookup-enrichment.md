<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > API Lookup enrichment

### Compatibility

JSON Schema	[iglu:com.snowplowanalytics.snowplow.enrichments/api_lookup_enrichment_config/jsonschema/1-0-0][schema]  
Compatibility	**NOT RELEASED YET**  
Data provider	Any  


### Overview

This enrichment uses any HTTP server to look up contexts related to particular event.

**TODO**

### Example

Here's factitious configuration that uses [OpenWeatherMap] [owm] to fetch weather weather conditions in which event has been occuried.

**WARNING** This configuration is for demonstration purposes only, as it uses very naive cache implementation and invalid time format.
For actual weather lookup you may want to check our [Weather enrichment](Weather-enrichment)


```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow.enrichments/api_lookup_enrichment_config/jsonschema/1-0-0",

    "data": {
        "enabled": true,
        "vendor": "com.snowplowanalytics.snowplow.enrichments",
        "name": "api_lookup_enrichment_config",
        "parameters": {
            "inputs": [
                {
                    "key": "latitude",
                    "pojo": {
                        "field": "geo_latitude"
                    }
                },
                {
                    "key": "longitude",
                    "pojo": {
                        "field": "geo_longitude"
                    }
                },
                {
                    "key": "time",
                    "pojo": {
                        "field": "derived_tstamp"
                    }
                },

            ],
            "api": {
                "http": {
                    "method": "GET",
                    "uri": "http://history.openweathermap.org/data/2.5/history/city?lon={{longitude}}&lat={{latitude}}&cnt=1&start={{time}}&appid=YOUR-OWM-API-KEY",
                    "timeout": 5000,
                    "authentication": { }
                }
            },
            "outputs": [ 
                {
                    "schema": "iglu:org.openweathermap/weather/jsonschema/1-0-0",
                    "json": {
                        "jsonPath": "$.list[0]",
                    }
                } 
            ],
            "cache": {
                "size": 3000,
                "ttl": 60
            }
        }
    }
}
```


[owm]: http://openweathermap.org
[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow.enrichments/api_lookup_enrichment_config/jsonschema/1-0-0
