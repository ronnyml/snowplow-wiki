[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 4: setting up alternative data stores**](Setting-up-alternative-data-stores) > [**1: Installing the StorageLoader**](1-Installing-the-StorageLoader)

1. [Assumptions](#assumptions)
2. [Dependencies](#dependencies)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Next steps](#next-steps)

<a name="assumptions" />
## 1. Assumptions

This guide assumes that you have administrator access to a Unix-based server (e.g. Ubuntu, OS X, Fedora) which you can install StorageLoader on.

<a name="dependencies"/>
## 2. Dependencies

### 2.1 Software

The StorageLoader jar is available for download. For more information, see the [[Hosted assets]] page.

The StorageLoader requires Java 7 to run.

Alternatively, to build the StorageLoader yourself, first make sure that you have **all** of the following installed:

1. **Git** - see the [Git Installation Guide] [git-install]
2. **Ruby and RVM*** - see our [Ruby and RVM setup guide](Ruby-and-RVM-setup)

\* If you prefer, an alternative Ruby manager such as chruby or rbenv should work fine too.

Also make sure that if you are loading Snowplow events into a PostgreSQL database, then the StorageLoader **must be run on the same server running PostgreSQL**. That is because it downloads the files locally, and Postgres needs to be able to ingest the data from the local file system.

<a name="s3-buckets"/>
### 2.2 S3 buckets

StorageLoader moves the Snowplow event files through three distinct S3 buckets during
the load process. These buckets are as follows:

1. **In Bucket** - contains the Snowplow event files to process
2. **Archive Bucket** - where StorageLower moves the Snowplow
   event files after successful loading

The In Bucket for StorageLoader is the same as the Out Bucket for the EmrEtlRunner -
i.e. you will already have setup this bucket.

We recommend creating a new folder for the Archive Bucket - i.e. do **not** re-use
EmrEtlRunner's own Archive Bucket. Create the required Archive Bucket in the same
AWS region as your In Bucket.

Right, now we can install StorageLoader.

<a name="installation"/>
## 3. Installation

To build the StorageLoader yourself, checkout the Snowplow repository and navigate to the StorageLoader root:

    $ git clone git://github.com/snowplow/snowplow.git
    $ cd snowplow/4-storage/storage-loader
    $ ./build.sh

Check it worked okay:

    $ ./deploy/snowplow-storage-loader --version
    snowplow-storage-loader 0.5.0

<a name="configuration"/>
## 4. Configuration

StorageLoader requires a YAML format configuration file to run. This should be the same file you use to configure the EmrEtlRunner. See [[Common configuration]] more information on its format.

<a name="next-steps" />
## 5. Next steps

All done? You have the StorageLoader installed! Now find out [how to use it](2-using-the-storageloader).

[git-install]: http://git-scm.com/book/en/Getting-Started-Installing-Git

[redshift-config-yml]: https://github.com/snowplow/snowplow/blob/master/4-storage/storage-loader/config/redshift.yml.sample
[postgres-config-yml]: https://github.com/snowplow/snowplow/blob/master/4-storage/storage-loader/config/postgres.yml.sample

[redshift-copy]: http://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html
