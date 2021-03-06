<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 5: Get started with data modeling**](getting-started-with-data-modeling)

[[https://d3i6fms1cm1j0i.cloudfront.net/github-wiki/images/snowplow-architecture-5-data-modeling.png]]

Once your data is stored in S3 and Redshift, the basic setup is complete. You now have access to the event stream: a long list of packets of data, where each packet represents a single event. While it is possible to do analysis directly on this event stream, it is common to:

1. Join event-level data with other data sets (e.g. customer data)
2. Aggregate event-level data into smaller data sets (e.g. sessions)
3. Apply business logic (e.g. user segmentation)

We call this process *data modeling*. The data model is usually developed in three stages.

## Step 1: Implement the standard data model

In a first step, the basic Snowplow data model is set up in so-called *full mode*. This creates a set of dervied tables in Redshift which are recomputed each time the pipeline runs, which allows for rapid iteration as long as date volumes are relatively small. The basic data model can be downloaded from [GitHub] [github-data-models] and the underlying logic explained in more detail in the [Analytics Cookbook] [analytics-cookbook] on the [Snowplow website] [snowplow-website].

## Step 2: Customize the SQL data model

The basic model can then be modified to include business-specific logic. This could mean adding ecommerce fields or aggregating events in different ways, ideally joining Snowplow data with other data sets (e.g. customer data).

## Step 3: Move to an incremental approach

Once the custom data model is in a stable state, i.e. all data needed to build reports has been added, the queries can be migrated from a *full* to an *incremental mode*. The incremental model updates the derived tables using only the most recent events, rather than recompute the tables each time using all events. The difference between both modes is described in more detail in the [data modeling section] [cookbook-data-modeling] of the [Analytics Cookbook] [analytics-cookbook].

## SQLRunner

The data model consists of a set of SQL queries which are executed in sequence using our [SQLRunner application] [sql-runner-github]. The order in which the queries are executed is determined by a config file, an example of which can be found on [GitHub] [github-data-models-config-example].

[analytics-cookbook]: http://snowplowanalytics.com/analytics/index.html
[cookbook-data-modeling]: http://snowplowanalytics.com/analytics/event-dictionaries-and-data-models/data-modeling.html
[snowplow-website]: http://snowplowanalytics.com
[github-data-models]: https://github.com/snowplow/snowplow/tree/master/5-data-modeling/sql-runner/redshift/
[github-data-models-config-example]: https://github.com/snowplow/snowplow/tree/master/5-data-modeling/sql-runner/redshift/playbooks/full.yml
[sql-runner-github]: https://github.com/snowplow/sql-runner