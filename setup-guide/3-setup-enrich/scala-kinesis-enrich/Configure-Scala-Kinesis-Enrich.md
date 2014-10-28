<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > [**Step 3.2: setting up Scala Kinesis Enrich**](Setting-up-Scala-Kinesis-Enrich) > [2: Configuring](Configuring-Scala-Kinesis-Enrich)

**This documentation is for version 0.2.0 of Scala Kinesis Enrich, which has not yet been released. Documentation for other versions is available:**

**[Version 0.1.0][v0.1]**

The Scala Stream Collector has a number of configuration options available.

## Basic configuration

### Template

Download a template configuration file from GitHub: [default.conf] [app-conf].

Now open the `default.conf` file in your editor of choice.

### AWS settings

Values that must be configured are:

+ `enrich.aws.access-key`
+ `enrich.aws.secret-key`

You can insert your actual credentials in these fields. Alternatively, if you set both fields to "env", your credentials will be taken from the environment variables AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.

### Source

The `enrich.source` setting determines which of the supported sources to read raw Snowplow events from:

+ `"kinesis`" for reading Thrift-serialized records from a named Amazon Kinesis stream
+ `"stdin`" for reading Base64-encoded Thrift-serialized records from the app's own `stdin` I/O stream

If you select `"kinesis"`, you need to set `enrich.streams.in` to the name of your raw Snowplow event stream [configured in your Scala Stream Collector](Configure-the-Scala-Stream-Collector).

### Sinks

The `enrich.sink` setting determines which of the supported sinks to write enriched Snowplow events to:

+ `"kinesis"` for writing enriched Snowplow events to a named Amazon Kinesis stream
+ `"stdouterr"` for writing enriched Snowplow events records to the app's own `stdout` I/O stream

If you select `"kinesis"`, you need to also update the `enrich.streams.out` section:

```
out: {
  enriched: "SnowplowEnriched"
  enriched_shards: 1 # Number of shards to use if created.
  bad: "SnowplowBad" # Not used until #463
  bad_shards: 1 # Number of shards to use if created.
}
```

Note that the Scala Kinesis Enrich does not yet support writing out bad rows to a dedicated Kinesis stream - so for now you can ignore those settings and simply configure the `enriched` and `enriched_shards` fields.

## Configuring enrichments

You may wish to use Snowplow's configurable enrichments. To do this, create a directory of enrichment JSONs. For each configurable enrichment you wish to use, the enrichments directory should contain a .json file with a configuration JSON for that enrichment. When you come to run Scala Kinesis Enrich you can then pass in the filepath to this directory using the --enrichments option.

Sensible default configuration enrichments are available on GitHub: [3-enrich/emr-etl-runner/config/enrichments][enrichment-json-examples].

See the documentation on [configuring enrichments][configuring-enrichments] for details on the available enrichments.

Next: [[Run Scala Kinesis Enrich]]

[v0.1]: https://github.com/snowplow/snowplow/wiki/Configure-Scala-Kinesis-Enrich-v0.1
[app-conf]: https://github.com/snowplow/snowplow/blob/master/3-enrich/scala-kinesis-enrich/src/main/resources/default.conf
[enrichment-json-examples]: https://github.com/snowplow/snowplow/tree/master/3-enrich/emr-etl-runner/config/enrichments
[configuring-enrichments]: https://github.com/snowplow/snowplow/wiki/5-Configuring-enrichments#template
