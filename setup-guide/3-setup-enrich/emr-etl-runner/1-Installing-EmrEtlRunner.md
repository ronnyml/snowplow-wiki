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

EmrEtlRunner requires a YAML format configuration file to run. There is a configuration file template available in the Snowplow GitHub repository at [`/3-enrich/emr-etl-runner/config/config.yml.sample`] [config-yml]. See [[Common configuration]] more information on how to write this file.

### iglu

You will also need an Iglu resolver configuration file. This is where we list the schema repositories to use to retrieve JSON Schemas for validation. For more information on this, see the [wiki page for Configuring shredding](5-Configuring-shredding).

<a name="enrichments" />
## 5. Configuring enrichments

If you wish to use Snowplow enrichments, see the [wiki page for configuring enrichments](5-Configuring-enrichments).

<a name="next-steps" />
## 6. Next steps

All done installing EmrEtlRunner? Then [learn how to use it] [using-emretlrunner]

[git-install]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[config-yml]: https://github.com/snowplow/snowplow/blob/master/3-enrich/emr-etl-runner/config/config.yml.sample
[using-emretlrunner]: 2-Using-EmrEtlRunner

[install-aws-cli]: http://docs.aws.amazon.com/cli/latest/userguide/installing.html