[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 4: setting up alternative data stores**](Setting-up-alternative-data-stores) > [**4: Loading shredded types**](4-Loading-shredded-types)

1. [Overview](#overview)
2. [Defining and installing a new table](#table)
3. [Creating and uploading a JSON Paths file](#jsonpaths)
4. [Configuring StorageLoader](#configure)
5. [Next steps](#next-steps)

<a name="overview"/>
## 1. Overview

Snowplow has a [Shredding process](Shredding) for Redshift which consists of two phases:

1. Extracting unstructured event JSONs and context JSONs from enriched event files into their own files
2. Loading those files into corresponding tables in Redshift

The second phase is instrumented by StorageLoader and is documented on this wiki page; to configure the first phase, visit the [Configuring shredding](5-Configuring-shredding) wiki page.

<a name="overview"/>
## 2. Defining and installing a new table

Each shredded type lives in its own table in Redshift. In this section, we will help you define and install these tables.

### For Snowplow-authored JSONs

Snowplow provides pre-made Redshift table definitions for all Snowplow-authored JSONs. You find these here:

    https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql

For example, if you have link click tracking enabled in the JavaScript Tracker, then add 

### For JSONs authored by you

You will need to write your own table definition for any JSON you create.

<a name="jsonpaths"/>
## 3. Creating and uploading a JSON Paths file

### For Snowplow-authored JSONs

There's nothing to do here - Snowplow already provides JSON Paths files for all the JSONs its supports out of the box. StorageLoader will automatically pick these JSON Paths files up and use them to load shredded types into Redshift.

### For JSONs authored by you

You will need to create your own JSON Paths file. The format is simple - a JSON Paths file consists of a JSON array, where each element corresponds to a column in the target table. To correspond to the table definition, each JSON Path file must start with the following elements:

```json
TODO
```

<a name="configure"/>
## 4. Configuring StorageLoader

Now you need to update StorageLoader's `config.yml` to load the shredded types. First, make sure that you have populated this section correctly:

```yaml
:shredded:
  :good: ADD HERE # Must be s3:// not s3n:// for Redshift. This is the same as the :shredded:good: bucket specified for EmrEtlRunner
  :archive: ADD HERE # Where to archive shredded types to
```

Make sure it cross-checks with the corresponding paths in your EmrEtlRunner's `config.yml`.

Next, let's xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.

<a name="next-steps"/>
## 5. Next steps

That's it for configuring StorageLoader for shredding. You should be ready to load shredded types into Redshift.
