<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > [**Step 3.1: setting up EmrEtlRunner**](Setting-up-EmrEtlRunner) > [**1: Installing EmrEtlRunner**](1-Installing-EmrEtlRunner) > [EmrEtlRunner input formats](EmrEtlRunner-Input-Formats)

Supported input formats for the EmrEtlRunner are as follows:

| **Input format**                              | **Use this when**                                                 | **Documentation**                       |
|:----------------------------------------------|:------------------------------------------------------------------|:----------------------------------------|
| `cloudfront`                                  | You are running the CloudFront Collector                          | [[Setting up the Cloudfront collector]] | 
| `clj-tomcat`                                  | You are running the Clojure Collector on Elastic Beanstalk        | [[Setting up the Clojure collector]]    |
| `thrift`                                      | You are using the Scala Stream Collector plus Kinesis LZO S3 Sink | [[Setting-up-the-Scala-Stream-Collector]] & [[Kinesis-LZO-S3-Sink-Setup]] |
| `tsv/com.amazon.aws.cloudfront/wd_access_log` | You are analyzing Amazon CloudFront access logs                   | [[Parsing CloudFront access logs]]      |

