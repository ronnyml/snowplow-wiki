<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > [**Step 3.1: setting up EmrEtlRunner**](Setting-up-EmrEtlRunner) > [1: Installing EmrEtlRunner](1-Installing-EmrEtlRunner)

1. [Assumptions](#assumptions)
2. [Dependencies](#dependencies)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Configuring enrichments](#enrichments)
6. [Next steps](#next-steps)

<a name="assumptions"/>
## 1. Assumptions

This guide assumes that you have administrator access to a Unix-based server (e.g. Ubuntu, OS X, Fedora) on which you can install EmrEtlRunner and schedule a regular cronjob.

_In theory EmrEtlRunner can be deployed onto a Windows-based server, using the Windows Task Scheduler instead of cron, but this has not been tested or documented._

<a name="dependencies"/>
## 2. Dependencies

### 2.1 Hardware

You will need to setup EmrEtlRunner on your own server. A number of people choose to do so on an EC2 instance (thereby keeping all of Snowplow in the Amazon Cloud). If you do so, please note that you **must not use a `t1.micro` instance**.  You should at the very least use an `m1.small` instance.

### 2.2 Software

To install EmrEtlRunner, first make sure that your server has **all** of the following installed:

1. **Git** - see the [Git Installation Guide] [git-install]
2. **Ruby and RVM*** - see our [Ruby and RVM setup guide](Ruby-and-RVM-setup). Both EmrEtlRunner and StorageLoader require Ruby 1.9.3

\* If you prefer, an alternative Ruby manager such as chruby or rbenv should work fine too.

### 2.3 EC2 key

You will also need an **EC2 key pair** setup in your Amazon EMR account.

For details on how to do this, please see the section "Configuring the client" in the [[Setting up EMR command line tools]] wiki page. Make sure that you setup the EC2 key pair inside the region in which you will be running your ETL jobs.

<a name="s3-buckets"/>
### 2.4 S3 locations

EmrEtlRunner processes data through three distinct states:

1. **:raw** - raw Snowplow event logs are the input to the EmrEtlRunner process
2. **:enriched** - EmrEtlRunner validates and enriches the raw event logs into enriched events
3. **:shredded** - EmrEtlRunner shreds JSONs found in enriched events ready for loading into dedicated Redshift tables

For `:raw:in`, specify the Amazon S3 path you configured for your Snowplow collector.

For all other S3 locations, you can specify paths within a single S3 bucket that you setup now. This bucket must be in the same AWS region as your `:raw:in` bucket.

Done? Right, now we can install EmrEtlRunner.

<a name="installation"/>
## 3. Installation

First, checkout the Snowplow repository and navigate to the EmrEtlRunner root:

    $ git clone git://github.com/snowplow/snowplow.git
    $ cd snowplow/3-enrich/emr-etl-runner
    
Next you are ready to install the application on your system:

    $ bundle install --deployment

Check it worked okay:

    $ bundle exec bin/snowplow-emr-etl-runner --version
    snowplow-emr-etl-runner 0.0.9

If you have any problems installing, please double-check that you have successfully completed our [Ruby and RVM setup guide](Ruby-and-RVM-setup).

<a name="configuration"/>
## 4. Configuration

EmrEtlRunner requires a YAML format configuration file to run. There is a configuration file template available in the Snowplow GitHub repository at [`/3-enrich/emr-etl-runner/config/config.yml.sample`] [config-yml]. The template looks like this:

```yaml
:logging:
  :level: DEBUG # You can optionally switch to INFO for production
:aws:
  :access_key_id: ADD HERE
  :secret_access_key: ADD HERE
:s3:
  :region: ADD HERE
  :buckets:
    :assets: s3://snowplow-hosted-assets # DO NOT CHANGE unless you are hosting the jarfiles etc yourself in your own bucket
    :log: ADD HERE
    :raw:
      :in: ADD HERE
      :processing: ADD HERE
      :archive: ADD HERE    # e.g. s3://my-archive-bucket/raw
    :enriched:
      :good: ADD HERE       # e.g. s3://my-out-bucket/enriched/good
      :bad: ADD HERE        # e.g. s3://my-out-bucket/enriched/bad
      :errors: ADD HERE     # Leave blank unless :continue_on_unexpected_error: set to true below
    :shredded:
      :good: ADD HERE       # e.g. s3://my-out-bucket/shredded/good
      :bad: ADD HERE        # e.g. s3://my-out-bucket/shredded/bad
      :errors: ADD HERE     # Leave blank unless :continue_on_unexpected_error: set to true below
:emr:
  :ami_version: 2.4.2      # Choose as per http://docs.aws.amazon.com/ElasticMapReduce/latest/DeveloperGuide/emr-plan-ami.html
  :region: ADD HERE        # Always set this
  :placement: ADD HERE     # Set this if not running in VPC. Leave blank otherwise
  :ec2_subnet_id: ADD HERE # Set this if running in VPC. Leave blank otherwise
  :ec2_key_name: ADD HERE
  :software:
    :hbase:                # To launch on cluster, provide version, "0.92.0", keep quotes
    :lingual:              # To launch on cluster, provide version, "1.1", keep quotes
  # Adjust your Hadoop cluster below
  :jobflow:
    :master_instance_type: m1.small
    :core_instance_count: 2
    :core_instance_type: m1.small
    :task_instance_count: 0 # Increase to use spot instances
    :task_instance_type: m1.small
    :task_instance_bid: 0.015 # In USD. Adjust bid, or leave blank for non-spot-priced (i.e. on-demand) task instances
:etl:
  :job_name: Snowplow ETL # Give your job a name
  :versions:
    :hadoop_enrich: 0.12.0 # Version of the Hadoop Enrichment process
    :hadoop_shred: 0.3.0 # Version of the Hadoop Shredding process
  :collector_format: cloudfront # Or 'clj-tomcat' for the Clojure Collector
  :continue_on_unexpected_error: false # Set to 'true' (and set :out_errors: above) if you don't want any exceptions thrown from ETL
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

To take each section in turn:

### logging

This section allows you to determine how verbose/chatty the log output from EmrEtlRunner should be.

### aws

The `aws` variables should be self-explanatory - enter your AWS access key and secret here.

### s3

The `region` variable should hold the AWS region in which your four data buckets (In Bucket, Processing Bucket etc) are located, e.g. "us-east-1" or "eu-west-1". Please note that Redshift can only load data from S3 buckets located in the same region as the Redshift instance, and Amazon has not to date launched Redshift in *every* region. So make sure that if you're using Redshift, the bucket specified here is in a region that supports Redshift.

Within the `s3` section, the `buckets` variables are as follows:

* `:assets:` holds the ETL job's static assets (HiveQL script plus Hive deserializer). You can leave this as-is (pointing to Snowplow   Analytics' [own public bucket containing these assets](Hosted-assets)) or replace this with your own private bucket containing the assets
* `:log:` is the bucket in which Amazon EMR will record processing information for this job run, including logging any errors  
* `:raw:` is where you specify the paths through which your raw Snowplow events will flow. For `:processing:`, **always include a sub-folder on this variable (see below for why)**. `:archive:` is where your raw Snowplow events will be moved after they have been successfully processed by Elastic MapReduce
* `:enriched:` is where you specify the paths through which your enriched Snowplow events will flow.
* `:shredded:` is where you specify the paths through which your shredded types will flow

For `:good:`, **always include a sub-folder on this variable (see below for why)**. If you are loading data into Redshift, the `:good:` specified here **must** be located in a region where Amazon has launched Redshift, because Redshift can only bulk load data from S3 that is located in the same region as the Redshift instance, and Redshift has not, to-date, been launched across all Amazon regions

Each of the bucket variables must start with an S3 protocol - either `s3://` or `s3n://`. Each variable can include a sub-folder within the bucket as required, and a trailing slash is optional.

The `:bad:` entries will store any raw Snowplow log lines which did not pass the enrichment or JSON validation, along with their validation errors. The `:errors:` entries will contain any raw Snowplow log lines which caused an unexpected error, but only if you set continue_on_unexpected_error to true (see below).

**Important 1:** there is a bug in Hive on Amazon EMR where Hive dies if you attempt to read or write data to the root of an S3 bucket. **Therefore always specify a sub-folder (e.g. `/events/`) for the `:raw:processing`, `:enriched:good` and `:shredded:good` locations.**

**Important 2:** do not put your `:raw:processing` inside your `:raw:in` bucket, or your `:enriched:good` inside your `:raw:processing`, or you will create circular references which EmrEtlRunner cannot resolve when moving files.

**Important 3:** if you are using the **Clojure collector**, the path to your `:raw:in` path will be of the format: 

	s3://elasticbeanstalk-{{REGION NAME}}-{{UUID}}/resources/environments/logs/publish/{{SECURITY GROUP IDENTIFIER}}

Replace all of these `{{x}}` variables with the appropriate ones for your environment (which you should have written down in the [Enable logging to S3](Enable-logging-to-S3) stage of the Clojure Collector setup).

Also - Clojure Collector usees should be sure not include an `{{INSTANCE IDENTIFIER}}` at the end of your path. This is because your Clojure Collector may end up logging into multiple `{{INSTANCE IDENTIFIER}}` folders. (If e.g. Elastic Beanstalk spins up more instances to run the Clojure collector, to cope with a spike in traffic.) By specifying your In Bucket only to the level of the Security Group identifier, you make sure that Snowplow can process all logs from all instances. (Because the EmrEtlRunner will process all logs in all subfolders.)

**Example bucket settings**

Here is an example configuration: 

```yaml
:buckets:
  :assets: s3://snowplow-hosted-assets
  :in: s3n://my-snowplow-logs/
  :log: s3n://my-snowplow-etl/logs/
  :raw:
    :in: s3n://my-snowplow-logs/
    :processing: s3n://my-snowplow-etl/processing/
    :archive: s3://my-archive-bucket/raw
  :enriched:
    :good: s3://my-data-bucket/enriched/good
    :bad: s3://my-data-bucket/enriched/bad
    :errors: s3://my-data-bucket/enriched/errors
  :shredded:
    :good: s3://my-data-bucket/shredded/good
    :bad: s3://my-data-bucket/shredded/bad
    :errors: s3://my-data-bucket/shredded/errors
```

Please note that all buckets must exist prior to running EmrEtlRunner; trailing slashes are optional.

### emr

The EmrEtlRunner makes use of Amazon Elastic Mapreduce (EMR) to process the raw log files and output the cleaned, enriched Snowplow events table.

This section of the config file is where we configure the operation of EMR. The variables with defaults can typically be left as-is, but you will need to set:

1. `region`, which is the Amazon EC2 region **and** availability zone
   in which the job should run, e.g. "us-east-1a" or "eu-west-1b"
2. `ec2_key_name`, which is the name of the Amazon EC2 key that you
   set up in the [Dependencies](#dependencies) above

Make sure that the EC2 key you specify belongs in the region you specify, or else EMR won't be able to find the key. **It's strongly recommended that you choose the same Amazon region as your S3 buckets are located in.**

Additionally, fill in one of these two:

*  `placement`, which is the Amazon EC2 region **and** availability zone
   in which the job should run, e.g. "us-east-1a" or "eu-west-1b"
*  `ec2_subnet_id`, which is the ID of the Amazon EC2 subnet you want
   to run the job in

You only need to set one of these (they are mutually exclusive settings), but you must set one.

The `:software:` section lets you start up Lingual and/or HBase when you start up your Elastic MapReduce cluster. This is the configuration to start up both, specifying the versions to start:

```yaml
:software:
  :hbase: "0.92.0"
  :lingual: "1.1"
```

### iglu

This is where we list the schema repositories to use to retrieve JSON Schemas for validation. For more information on this, see the [wiki page for Configuring shredding](5-Configuring-shredding).

### etl

This section is where we configure exactly how we want our ETL process to operate:

1. `job_name`, the name to give our ETL job. This makes it easier to identify your ETL job in the Elastic MapReduce console
2. `hadoop_etl_version` is the version of the Hadoop ETL process to run. This variable lets you upgrade the ETL process without having to update the EmrEtlRunner application itself
3. `collector_format`, what format is our collector saving data in? Currently three formats are supported: "cloudfront" (if you are running the Cloudfront collector), "clj-tomcat" if you are running the Clojure collector, or "thrift" if you are using the Scala Stream Collector plus Kinesis LZO S3 Sink
4. `continue_on_unexpected_error`, continue processing even on unexpected row-level errors, e.g. an input file not matching the expected CloudFront format. Off ("false") by default

<a name="enrichments" />
## 5. Configuring enrichments

If you wish to use Snowplow enrichments, see the [wiki page for configuring enrichments](Configuring-enrichments).

<a name="next-steps" />
## 6. Next steps

All done installing EmrEtlRunner? Then [learn how to use it] [using-emretlrunner]

[git-install]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[config-yml]: https://github.com/snowplow/snowplow/blob/master/3-enrich/emr-etl-runner/config/config.yml.sample
[using-emretlrunner]: 2-Using-EmrEtlRunner
