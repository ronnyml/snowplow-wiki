Snowplow has a Shredding process which consists of two phases:

1. Extracting unstructured event JSONs and context JSONs from enriched event files into their own files
2. Loading those files into corresponding tables in Redshift

Postgres does not support copying data in JSON format, so you cannot load Snowplow JSONs into Postgres. However, if you are using Postgres, you must still run the shred step because it also creates the TSV which is loaded into the atomic.events table.

## Technical architecture

The shredding flow and main components are highlighted in blue on the right-hand side of this technical architecture:

[[/technical-documentation/images/shredding-architecture.png]]

## Main components

### 0. Iglu

Iglu is a schema repository technology which holds the JSON Schemas against which unstructured events and context JSONs are validated.

For more information on Iglu, see the [Iglu wiki] [iglu-wiki].

### 1. Scala Hadoop Shred

Scala Hadoop Shred is a dedicated Scalding job to perform the JSON validation and extraction. This is a five step process:

1. Reads Snowplow enriched events from S3
2. Extracts any unstructured event JSONs and context JSONs found
3. Validates that these JSONs conform to schema
4. Adds metadata to these JSONs to track their origins
5. Writes these JSONs out to nested folders dependent on their schema

Configuring this is covered in [Configuring shredding](6-Configuring-shredding).

### 2. StorageLoader

The StorageLoader has functionality to load shredded types into corresponding tables in Redshift, using Redshift's native `COPY FROM JSON` functionality. This is a multi-step process:

* Find folders of shredded types in S3
* For each folder of shredded types:
  * Find the JSON Paths file that corresponds to the shredded type
  * Determine the Redshift tablename from the shredded type
  * Load the shredded type files into the Redshift table using the JSON Paths file

Configuring this is covered in [Loading shredded types](4-Loading-shredded-types).

[iglu-wiki]: https://github.com/snowplow/iglu/wiki
