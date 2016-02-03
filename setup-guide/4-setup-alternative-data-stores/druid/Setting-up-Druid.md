<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 4: setting up alternative data stores**](Setting-up-alternative-data-stores) > Setup Druid

1. [Assumptions](#assumptionss)
2. [Prerequisites](#prerequisites)  
    2.1 [Amazon S3](#prereq-s3)
    2.2 [Postgres RDS](#prereq-postgres-rds)
    2.3 [ZooKeeper](#prereq-zookeeper)
    2.4 [Java 7](#prereq-java7)
3. [Download Druid](#download)  
3. [xxx](#zzz)

<a name="assumptions" />

1. Assumptions

This guide makes the following assumptions:

1. That you are using **Druid 0.8.3 Stable** - this is the version that we have tested these instructions with
2. That you are deploying Druid onto **AWS** - we have chosen the Druid dependencies which require the least configuration 

<a name="prerequisites" />

## 1. Prerequisites

The prerequisites for Druid are as follows:

* [Amazon S3] [amazon-s3] to act as the data repository for Druid ("deep storage")
* [Postgres on Amazon RDS] [pg-rds] to act as the metadata storage for Druid
* [Apache ZooKeeper] [zookeeper] to coordinate the Druid clusters
* [Java 7 or higher] [jre] on each of the cluster machines

Let's configure/install each of these in turn.

<a name="prerequisites" />

## 1. Prerequisites

<a name="prereq-s3" />
### 1.1 Amazon S3

We will use Amazon S3 as the data repository for Druid ("deep storage").

ADD REST OF SECTION

<a name="prereq-postgres-rds" />
### 1.2 Postgres RDS

We will use a PostgreSQL instance running on Amazon RDS as the medata storage for Druid.

ADD REST OF SECTION

<a name="prereq-zookeeper" />
### 1.3 ZooKeeper

We will use Apache ZooKeeper as the cluster coordination service for Druid.

**Setting up and running a production ZooKeeper cluster is out of the scope of this documentation. We strongly recommend reading [ZooKeeper (O'Reilly)] [zookeeper-oreilly] before proceeding.**

```
curl http://www.gtlib.gatech.edu/pub/apache/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz -o zookeeper-3.4.6.tar.gz
sudo tar xzf zookeeper-3.4.6.tar.gz -C /opt
cd /opt/zookeeper-3.4.6
sudo cp conf/zoo_sample.cfg conf/zoo.cfg
sudo ./bin/zkServer.sh start
```

<a name="prereq-java7" />
### 1.4 Java 7 of higher

Each of your cluster machines must be running Java 7 or higher:

```
$ java -version
java version "1.7.0_67"
Java(TM) SE Runtime Environment (build 1.7.0_67-b01)
Java HotSpot(TM) 64-Bit Server VM (build 24.65-b04, mixed mode)
```

<a name="download" />
## 3. Download Druid

Download the Druid binaries:

    $ wget http://static.druid.io/artifacts/releases/druid-0.8.3-bin.tar.gz
    $ tar -xvf druid-0.8.3-bin.tar.gz -C /opt
    $ cd /opt/druid-0.8.3 && ls
    LICENSE               extensions-repo       run_example_client.sh
    config                lib                   run_example_server.sh
    examples              run_druid_server.sh   select_example.sh

Launch a Redshift Cluster


[emr-etl-runner]: 1-Installing-EmrEtlRunner
[storage-loader]: 1-Installing-the-StorageLoader
[sql-workbench-tutorial]: http://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html
[redshift-table-def]: https://github.com/snowplow/snowplow/blob/master/4-storage/redshift-storage/sql/atomic-def.sql

[zookeeper]: https://en.wikipedia.org/wiki/Apache_ZooKeeper
[zookeeper-oreilly]: http://shop.oreilly.com/product/0636920028901.do

