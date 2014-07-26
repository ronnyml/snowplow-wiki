[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > [**Step 3.1: setting up EmrEtlRunner**](Setting-up-EmrEtlRunner) > [5: Configuring shredding](5-Configuring-shredding)

1. [Overview](#overview)
2. [Pre-requisites](#pre-reqs)
3. [Configuring EmrEtlRunner](#configure)
4. [Next steps](#next-steps)

<a name="overview"/>
## 1. Overview

Snowplow has a [Shredding process](Shredding) for Redshift which consists of two phases:

1. Extracting unstructured event JSONs and context JSONs from enriched event files into their own files
2. Loading those files into corresponding tables in Redshift

The first phase is instrumented by EmrEtlRunner; in this page we will explain how to configure the shredding process to operate smoothly with EmrEtlRunner.

**Note: this guide is ONLY required if you want to shred your own unstructured event JSONs and context JSONs. If you are only shredding Snowplow-authored JSONs like link clicks and ad impressions, then you can skip this page and go straight to [Loading shredded types](4-Loading-shredded-types).**

<a name="pre-reqs"/>
## 2. Pre-requisites

First off, we assume that all JSONs you are sending as unstructured events and contexts are self-describing JSONs. To find out more about self-describing JSONs:

* [Iglu documentation on self-describing JSONs](https://github.com/snowplow/iglu/wiki/Self-describing-JSONs)
* [JavaScript Tracker 2.0.0 release on self-describing JSONs](http://snowplowanalytics.com/blog/2014/07/03/snowplow-javascript-tracker-2.0.0-released/#schemas)
* [SchemaVer for semantic schema versioning](https://github.com/snowplow/iglu/wiki/SchemaVer)

Secondly, we assume that you have defined self-describing JSON Schemas for each of your JSONs. Resources:

* [Iglu documentation on self-describing JSON Schemas](https://github.com/snowplow/iglu/wiki/Self-describing-JSON-Schemas)
* [Understanding JSON Schema (PDF)](http://spacetelescope.github.io/understanding-json-schema/UnderstandingJSONSchema.pdf)

Thirdly, we assume that you have setup your own Iglu schema repository to host your schemas. Resources:

* [Setup a static Iglu repository](https://github.com/snowplow/iglu/wiki/Static-repo-setup)
* [Iglu FAQ: How do I host a schema privately?](https://github.com/snowplow/iglu/wiki/Developer-FAQ#how-do-i-host-a-schema-privately)

You are now ready to configure EmrEtlRunner for shredding.

<a name="configure"/>
## 3. Configuring EmrEtlRunner

The first relevant section of the EmrEtlRunner's `config.yml` is:

```yaml
:shredded:
  :good: ADD HERE       # e.g. s3://my-out-bucket/shredded/good
  :bad: ADD HERE        # e.g. s3://my-out-bucket/shredded/bad
  :errors: ADD HERE     # Leave blank unless :continue_on_unexpected_error: set to true below
```

Please make sure that these shredded buckets are set correctly. 

Next, we let EmrEtlRunner know about your Iglu schema repository, so that schemas can be retrieved from there as well as from Iglu Central. The relevant section of `config.yml` is:

```yaml
:iglu:
  :schema: iglu:com.snowplowanalytics.iglu/resolver-config/jsonschema/1-0-0
  :data:
    :cache_size: 500
    :repositories:
      - :name: "Iglu Central"
        :priority: 0
        :vendor_prefixes:
          - com.snowplowanalytics
        :connection:
          :http:
            :uri: http://iglucentral.com
```

You must add an extra entry in the `:repositories:` array pointing to your own Iglu schema repository.

For more information on how to do this, please review the [Iglu client configuration](https://github.com/snowplow/iglu/wiki/Iglu-client-configuration) wiki page. The EmrEtlRunner converts the YAML format given above into an Iglu client configuration JSON automatically.

Your updated `config.yml` will end up looking something like:

```yaml
:iglu:
  :schema: iglu:com.snowplowanalytics.iglu/resolver-config/jsonschema/1-0-0
  :data:
    :cache_size: 500
    :repositories:
      - :name: "Iglu Central"
        :priority: 0
        :vendor_prefixes:
          - com.snowplowanalytics
        :connection:
          :http:
            :uri: http://iglucentral.com
      - :name: "Acme's Iglu repository"
        :priority: 0
        :vendor_prefixes:
          - com.acme
        :connection:
          :http:
            :uri: http://internal.acme.com/iglu
```

<a name="next-steps"/>
## 4. Next steps

That's it for configuring EmrEtlRunner for shredding. Next, please refer to the [Loading shredded types](4-Loading-shredded-types) wiki page to understand how to configure StorageLoader.
