<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > JavaScript script enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/javascript_script_config/jsonschema/1-0-0][schema]  
Compatibility r66+
Data provider None

### Overview

This enrichment lets you write a JavaScript function which is executed in the Enrichment process for each enriched event, and returns one or more _derived contexts_ which are attached to the final enriched event.

Use this enrichment to apply your own business logic to your enriched events at the row-level (JavaScript functions across multiple rows are not supported). Because your JavaScript function can throw exceptions which are gracefully handled by the calling Enrichment process, you can also use this enrichment to provide simple filtering of events.

This is the field which this enrichment will augment:

* derived_contexts

### Example

#### JavaScript function

Your JavaScript must include a function, `process(event)`, which:

* Takes a [Snowplow enriched event POJO] [enriched-event-pojo] (Plain Old Java Object) as its sole argument
* Returns an array of valid self-describing JSONs, which will be added to the `derived_contexts` field in the enriched event
* Returns `[]` or `null` if there are no contexts to add to this event
* Can `throw` exceptions but note that throwing an exception will cause the entire enriched event to end up in the Bad Bucket

Note that you can also include other top-level functions and variables in your JavaScript script - but you must include a `process(event)` function somewhere in your script.

Here is an example:

```javascript
var xxx = xxx;

function process(event) {
    var appId = event.getApp_id();

    if (appId == null) {
        return [];
    }

    var appIdUpper = new String(appId.toUpperCase());
    return [ { schema: "iglu:com.acme/updated_app_id/jsonschema/1-0-0",
               data:  { appIdUpper: appIdUpper } } ];
}
```

Please note:

* You must use a Java-style getter to retrieve the `app_id` from the event ([more information] [rhino-experiments])
* We have to convert the uppercased `appId` back to a JavaScript String (from a Java String) before we return it ([more information] [string-gotcha])

#### JSON configuration file

The self-describing JSON to configure this enrichment is as follows:

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/javascript_script_config/jsonschema/1-0-0",
    "data": {
        "vendor": "com.snowplowanalytics.snowplow",
        "name": "javascript_script_config",
        "enabled": true,
        "parameters": {
            "script": "ZnVuY3Rpb24gcHJvY2VzcyhldmVudCkgew0KDQogIHZhciBhcHBJZCA9IGV2ZW50LmdldEFwcF9pZCgpOw0KICANCiAgaWYgKGFwcElkID09IG51bGwpIHsNCiAgICByZXR1cm4gW107DQogIH0NCg0KICAvLyBVc2UgbmV3IFN0cmluZygpIGJlY2F1c2UgaHR0cDovL25lbHNvbndlbGxzLm5ldC8yMDEyLzAyL2pzb24tc3RyaW5naWZ5LXdpdGgtbWFwcGVkLXZhcmlhYmxlcy8NCiAgdmFyIGFwcElkVXBwZXIgPSBuZXcgU3RyaW5nKGFwcElkLnRvVXBwZXJDYXNlKCkpOw0KDQogIHJldHVybiBbIHsgc2NoZW1hOiAiaWdsdTpjb20uc25vd3Bsb3dhbmFseXRpY3Muc25vd3Bsb3cvcmVtb3ZlX2Zyb21fY2FydC9qc29uc2NoZW1hLzEtMC0wIiwNCiAgICAgICAgICAgICAgIGRhdGE6IHsgc2t1OiBhcHBJZFVwcGVyLCBxdWFudGl0eTogMiB9DQogICAgICAgICAgIH0gXTsNCn0NCg=="
        }
    }
}
```

The "parameters" fields are as follows:

* "script": Your JavaScript function, Base64 encoded. You can use either URL-safe or regular Base64 encoding

### How this enrichment works

This enrichment uses the [Rhino JavaScript engine] [rhino] to execute your JavaScript. Your JavaScript is pre-compiled so your code should approach Java speeds.

The `process` function is passed the exact [Snowplow enriched event POJO] [enriched-event-pojo]. The return value from the `process` function is converted into a JSON string (using `JSON.stringify) before being retrieved in our Scala code.

You can review the exact code which executes your JavaScript script in this Scala file: [.scala] [enrichment-scala].

### Do's and Don'ts

This is our most powerful enrichment yet - here are some do's and don'ts to avoid potential footguns.

Do:

* return as many contexts as you want
* throw an exception if you want this enriched event to end up in the Bad Bucket
* include **minified, self-contained** JavaScript libraries that your `process(event)` function needs
* test this enrichment on sample sets of events before putting it into production
* ensure your new contexts are defined in Iglu, Redshift, JSON Paths etc

Don't:

* mutate existing fields in the supplied enriched event - return a new context instead
* try to share state across multiple enriched events - write your own Scalding or Spark job instead
* include CPU-intensive tasks without being aware of the impact on your event processing time

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/javascript_script_config/jsonschema/1-0-0

[rhino]: https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino
[enriched-event-pojo]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/outputs/EnrichedEvent.scala

[string-gotcha]: http://nelsonwells.net/2012/02/json-stringify-with-mapped-variables/
[rhino-experiments]: http://snowplowanalytics.com/blog/2013/10/21/scripting-hadoop-part-1-adventures-with-scala-rhino-and-javascript/
