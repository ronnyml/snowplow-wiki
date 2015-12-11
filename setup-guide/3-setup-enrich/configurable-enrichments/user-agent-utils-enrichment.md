<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > user-agent-utils enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/user_agent_utils_config/jsonschema/1-0-0][schema]  
Compatibility r63+  
Data provider [user-agent-utils][user-agent-utils]  

### Overview

This enrichment uses the [user-agent-utils][user-agent-utils] library to parse the useragent into the following fields:

br_name
br_family
br_version
br_type
br_renderengine
os_name
os_family
os_manufacturer
dvce_type
dvce_ismobile

### Example

This enrichment has no special configuration: it is either off or on. Enable it with the following JSON:

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/user_agent_utils_config/jsonschema/1-0-0",
    "data": {
        "vendor": "com.snowplowanalytics.snowplow",
        "name": "user_agent_utils_config",
        "enabled": true,
        "parameters": {}
  }
}
```

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/user_agent_utils_config/jsonschema/1-0-0
[user-agent-utils]: https://github.com/HaraldWalker/user-agent-utils
