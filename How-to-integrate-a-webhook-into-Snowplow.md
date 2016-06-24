### What is a WebHook
A WebHook is an HTTP callback: an HTTP POST that occurs when something happens; a simple event-notification via HTTP POST. A web application implementing WebHooks will POST a message to a URL when certain things happen.

WebHooks make it easier to integrate two systems. Example:
Each time a service status changes, the WebHook will POST some simple attributes to whatever URL you specify.

### WebHooks in Snowplow
All WebHooks are located inside the registry directory:

`3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry/`


There are a few different phases to write a WebHook:

### Exploration

* Sign up for the service whose webhooks you will be integrating
* Browse to the service's admin UI
* Switch on webhooks (sometimes called a Streaming or HTTP Response API)
* Provide a Snowplow collector as the target for the webhooks. If you work for Snowplow, you can use http://trial-collector.snplow.com/ For more real-time testing, setup a local collector and then use Ngrok
* Cause all the various webhook to be triggered, and review their payload structures

### Tickets

(You can skip this step if you don't work for Snowplow Analytics.)

Create three tickets for each webhook you will be supporting:

1. One ticket for the new JSON Schema in `snowplow/iglu-central`
2. One for the new Redshift DDL in `snowplow/snowplow`
3. One for the new JSON Paths file in `snowplow/snowplow`

Also create a ticket in `snowplow/snowplow` called "Scala Common Enrich: add Adapter to pre-process <<Service>> events".

### Integration work
* Add the new adapter to the **AdapterRegistry.scala** file: 

`3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/AdapterRegistry.scala`

The vendor name need to be added to the Vendors list.
  ```scala
  private object Vendor {
    ...
    val NewService = "com.newservice"
  }
  ```

Also, add the vendor name and new adapter filename to the **toRawEvents** method.
  ```scala 
  def toRawEvents... {
    ...
	  case (Vendor.NewService,  "v1")  => NewServiceAdapter.toRawEvents(payload)
	  ...
  }
  ```

* Then, create the **NewServiceAdapter** file into the registry directory:

`3-enrich/scala-common-enrich/src/main/scala/com.snowplowanalytics.snowplow.enrich/common/adapters/registry`

All adapters extend the main Adapter:
  ```scala 
  object NewServiceAdapter extends Adapter {
    ...
  }
  ```

* In the adapter, ensure that all data transformations are safe. Validate things like if the payload body is empty, if it is not a query string or if the payload query string is malformed.

* After the new adapter was written, a test is needed to verify that the adapter is working fine. Tests are located inside this directory: 

`3-enrich/scala-common-enrich/src/test/scala/com.snowplowanalytics.snowplow.enrich.common/adapters/registry/`

* Create a new test file called **NewServiceAdapterSpec**.

* Inside the test file, consider both, success and failure cases. This is extremely important to know when data is not comming as expected and to test all scenarios.

* To test only a single adapter we use the command **test-only** inside the directory `3-enrich/scala-common-enrich`:
```bash
$ test-only com.snowplowanalytics.snowplow.enrich.common.adapters.registry.NewServiceAdapterSpec
```

* Then, create a Hadoop test within this directory: 
`3-enrich/scala-hadoop-enrich/src/test/scala/com.snowplowanalytics.snowplow.enrich.hadoop/good/`  
This is a new test for ensuring that the adapter works with the batch based enrichment process.

### JSON schema, JSONpath and Redshift ddl files

* We can write these files manually or using a tool built by Snowplow called Schema Guru. The tool with full instructions could be found [here] [schema-guru]:

* The advantage of using schema-guru is that we save time and avoid errors.  

Download the latest Schema Guru from Bintray:
```bash
$ wget http://dl.bintray.com/snowplow/snowplow-generic/schema_guru_0.6.2.zip
$ unzip schema_guru_0.6.2.zip
```

* Once schema-guru is installed, start creating the schema, jsonpath and redshift files. A JSON file or a directory containing JSON instances as an input is needed.

Basic schema output:
```bash
$ ./schema-guru-0.6.2 schema file.json
```

Generate a schema file:
```bash
$ ./schema-guru-0.6.2 schema --output jsonschema file.json
```

Generate a schema file without length limits:
```bash
$ ./schema-guru-0.6.2 schema --output jsonschema file.json --no-length
```

Generate a redshift ddl from a directory containing a schema file:
```bash
$ ./schema-guru-0.6.2 ddl com.newservice
```

Generate a redshift ddl and JSON path from a directory containing a schema file:
```bash
$ ./schema-guru-0.6.2 ddl com.newservice --with-json-paths
```

[schema-guru]: https://github.com/snowplow/schema-guru