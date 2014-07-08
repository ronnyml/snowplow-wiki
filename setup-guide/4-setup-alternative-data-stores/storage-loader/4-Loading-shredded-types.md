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

<a name="snowplow-jsons"/>
## 2. Loading Snowplow-authored JSONs

Loading Snowplow-authored JSONs is straightforward: Snowplow provides pre-made Redshift table definitions for all Snowplow-authored JSONs. You can find these here:

    https://github.com/snowplow/snowplow/tree/master/4-storage/redshift-storage/sql

For example, if you have link click tracking enabled in the JavaScript Tracker, then install `com.snowplowanalytics.snowplow/link_click_1.sql` into your Snowplow database.

Each table needs to be loaded using a JSON Paths file. Snowplow provides JSON Paths files for all Snowplow-authored JSONs. StorageLoader will automatically locate these JSON Paths files and use them to load shredded types into Redshift.

<a name="overview"/>
## 3. Defining and installing a new table

StorageLoader loads each shredded type into its own table in Redshift. In this section, we will help you to create a table for a new shredded type you have defined.

Each table definition starts with a set of standard "boilerplate" fields. These fields help to document the type hierarchy which has been shredded and are very useful for later analysis. These fields are as follows:

```sql

```

<a name="jsonpaths"/>
## 4. Creating and uploading a JSON Paths file

### 4.1 Overview

You need to create a JSON Paths file which StorageLoader will use to load your shredded type into Redshift. The format is simple - a JSON Paths file consists of a JSON array, where each element corresponds to a column in the target table. For full details, see the [Copy from JSON](http://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-copy-from-json.html) documentation from Amazon.

### 4.2 Creating a JSON Paths file

To correspond to the table definition, each JSON Path file must start with the following elements:

```json
{
  "jsonpaths": [
  
    "$.schema.vendor",
    "$.schema.name",
    "$.schema.format",
    "$.schema.version",

    "$.hierarchy.rootId",
    "$.hierarchy.rootTstamp",
    "$.hierarchy.refRoot",
    "$.hierarchy.refTree",
    "$.hierarchy.refParent",
    ...
```

Then finish your array of JSON Paths with an element for each custom field in your table. For example, here are the remaining fields for the Snowplow link click event:

```json
	...
    "$.data.elementId",
    "$.data.elementClasses",
    "$.data.elementTarget",
    "$.data.targetUrl"
    ]
}
```

A few things to note:

* Scala Hadoop Shred will nest all of your JSON's properties into a `data` property, hence the namespacing seen above
* Currently the Shredding process does not support nested tables. A nested property such as an array (like `elementClasses` above) should be loaded into a single field

### 4.3 Naming the JSON Paths file

The JSON Paths file should be named the same as the table created in 3, **minus** the shredded type's vendor. For example, if your table is called:

    com_acme_website_customer_1

Then your JSON Paths file should be called:

    customer_1.json

### 4.4 Installing the JSON Paths file

Upload the JSON Paths file to a private S3 bucket which is accessible using the AWS credentials provided in your StorageLoader's `config.yml` file.

Store the JSON Paths file in a sub-folder named after the vendor, for example:

    s3://acmes-jsonpaths-files/com.acme.website/customer_1.json

<a name="configure"/>
## 5. Configuring StorageLoader

Now you need to update StorageLoader's `config.yml` to load the shredded types. First, make sure that your `:jsonpath_assets:` points to the private S3 bucket you stored the JSON Paths file in section 4.

```yaml
:buckets:
  :jsonpath_assets: s3://acmes-jsonpaths-file
```

Next, make sure that you have populated the `:shredded:` section correctly:

```yaml
:shredded:
  :good: ADD HERE # Must be s3:// not s3n:// for Redshift. This is the same as the :shredded:good: bucket specified for EmrEtlRunner
  :archive: ADD HERE # Where to archive shredded types to
```

Make sure it cross-checks with the corresponding paths in your EmrEtlRunner's `config.yml`.

<a name="next-steps"/>
## 5. Next steps

That's it for configuring StorageLoader for shredding. You should be ready to load shredded types into Redshift.
