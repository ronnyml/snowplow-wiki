<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configuring enrichments

Snowplow offers the option to configure certain enrichments. This is done using configuration JSONs. When running EmrEtlRunner, the `--enrichments` argument should be populated with the filepath of a directory containing your configuration JSONs. Each enrichment JSON file should be named {{name of enrichment}}.json (for example, referer_parser.json).

See [this page](Configurable-enrichments) for a list of the configurable enrichments.
