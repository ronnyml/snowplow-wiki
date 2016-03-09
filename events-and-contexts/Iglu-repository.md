[**HOME**](Home) » [**EVENTS AND CONTEXTS**](Events-and-Contexts) » [Event dictionary](Event-dictionary) » Iglu repository

##Overview

**Iglu** is a machine-readable, open-source schema repository for [JSON Schema](http://json-schema.org/) from the team at [Snowplow Analytics](http://snowplowanalytics.com/). A schema repository (sometimes called a schema registry) is like npm or Maven or git but holds data schemas instead of software or code.

Iglu consists of three key technical aspects:

1. A [common architecture](https://github.com/snowplow/iglu/wiki/Common-architecture) that informs all aspects of Iglu
2. [Iglu clients](https://github.com/snowplow/iglu/wiki/Iglu-clients) that can resolve schemas from one or more Iglu repositories
3. [Iglu repositories](https://github.com/snowplow/iglu/wiki/Iglu-repositories) that can host a set of [self-describing JSON Schemas](http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/)

##Iglu explained

**Iglu** is built on a set of technical design decisions. It is this set of design decisions that allow Iglu clients and repositories to interoperate. Please review the following design documents:

- [**Self-describing JSON Schema**](https://github.com/snowplow/iglu/wiki/Self-describing-JSON-Schemas) - simple extensions to JSON Schema which semantically identify and version a given JSON Schema
- [**Self-describing JSON**](https://github.com/snowplow/iglu/wiki/Self-describing-JSONs) - a standardized JSON format which co-locates a reference to the instance's JSON Schema alongside the instance's data
- [**SchemaVer**](https://github.com/snowplow/iglu/wiki/SchemaVer) - how we semantically version schemas
Schema resolution - our public algorithm for how we determine in which order we check Iglu repositories for a given schema

**Iglu clients** are used for interacting with Iglu server repos and for resolving schemas in embedded and remote Iglu schema repositories.

In the below diagram we show an Iglu client resolving a schema from Iglu Central, one embedded repository and a further two remote HTTP repositories:

![Iglu client](https://github.com/snowplow/iglu/wiki/technical-documentation/images/iglu-clients.png)

An **Iglu repository** acts as a store of data schemas (currently JSON Schemas only). Hosting JSON Schemas in an Iglu repository allows you to use those schemas in Iglu-capable systems such as [Snowplow](https://github.com/snowplow/snowplow/wiki/).

So far we support two types of Iglu repository:

- **Remote repositories** - essentially websites containing schemas which an Iglu client can query over HTTP
- **Embedded repositories** - which are embedded in a piece of software (typically alongside an Iglu client)

In the below diagram we show an Iglu client resolving a schema from Iglu Central, one embedded repository and a further two remote HTTP repositories:

![Iglu repositories](https://github.com/snowplow/iglu/wiki/technical-documentation/images/iglu-repos.png)

**Iglu Central** (http://iglucentral.com) is a public repository of JSON Schemas hosted by [Snowplow Analytics](http://snowplowanalytics.com/).

Under the covers, Iglu Central is built and run as a **static Iglu repository**, hosted on Amazon S3.

> A **static repo** is simply an Iglu repository server structured as a static website.

![Iglu Central](https://github.com/snowplow/iglu/wiki/technical-documentation/images/iglu-central.png)

The **deployment process** for Iglu Central is documented on [this wiki](https://github.com/snowplow/iglu/wiki/Iglu-Central-setup) in case a user wants to setup a public mirror or private instance of Iglu Central.

###Further reading

To find out more about the concepts mentioned above and ultimately how to set up custom events and contexts and send them to Snowplow pipeline, follow the links below.

- [Iglu common architecture](https://github.com/snowplow/iglu/wiki/Common-architecture)
- [Iglu clients](https://github.com/snowplow/iglu/wiki/Iglu-clients)
- [Iglu repositories](https://github.com/snowplow/iglu/wiki/Iglu-repositories)
- [Iglu Central](https://github.com/snowplow/iglu/wiki/Iglu-Central-setup)
- [Self-describing JSON](https://github.com/snowplow/iglu/wiki/Self-describing-JSONs)
- [Self-describing JSON Schema](http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/)
- [SchemaVer](https://github.com/snowplow/iglu/wiki/SchemaVer)
- [Custom events](Custom-events)
- [Custom-contexts](Custom-contexts)
- [Event dictionary](Event-dictionary)
