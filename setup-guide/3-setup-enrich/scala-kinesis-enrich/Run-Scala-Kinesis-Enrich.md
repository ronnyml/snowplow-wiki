<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > [**Step 3.2: setting up Scala Kinesis Enrich**](Setting-up-Scala-Kinesis-Enrich) > [3: Running](Running-Scala-Kinesis-Enrich)

**This documentation is for version 0.2.1 of Scala Kinesis Enrich, which has not yet been released. Documentation for other versions is available:**

**[Version 0.1.0][v0.1]**

## Running

Scala Kinesis Enrich is an executable jarfile which should be runnable from any Unix-like shell environment. Simply provide the configuration file as a parameter:

    $ ./scala-kinesis-enrich-0.2.1 --config my.conf

This will start the Scala Kinesis Enrich app to read raw events from Kinesis and write enriched events back to Kinesis.

If you are using configurable enrichments, provide the path to your enrichments directory as a parameter:

    $ ./scala-kinesis-enrich-0.2.1 --config my.conf --enrichments path/to/enrichments

## All done?

You have setup Scala Kinesis Enrich! You are now ready to [setup alternative data stores](Setting-up-alternative-data-stores).

Return to the [setup guide](Setting-up-Snowplow).

[v0.1]: https://github.com/snowplow/snowplow/wiki/Run-Scala-Kinesis-Enrich-v0.1
