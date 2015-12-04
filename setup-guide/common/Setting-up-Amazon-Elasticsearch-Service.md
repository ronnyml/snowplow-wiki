[**HOME**](Home) > [**Setting up Amazon Elasticsearch Service**](Setting-up-Amazon-Elasticsearch-Service)

#### Overview

If an event fails the Snowplow enrichment process, it wil be written to an S3 bucket for bad rows together with an error message explaining what went wrong. The [EmrEtlRunner](Setting-up-EmrEtlRunner) supports loading these bad events into an Elasticsearch database to be examined more easily.

This guide describes how to set up and [Amazon Elasticsearch Service][aes] domain to store bad rows.

#### Getting started

Go to the AWS console and select "Elastic Service" from the list of services.

Click on the "Create a new domain" button.

#### Name domain

Choose a descriptive name for your Elasticsearch domain. Note that this name will not be your final domain name - that won't be finalized until the domain is actually created.

Click the "Next" button.

#### Configure cluster

The default cluster configuration options are all fine, but you may want to increase the instance count to make your domain more fault-tolerant.

Click the "Next" button.

#### Set up access policy

From the "Select a template" drop-down menu, choose "Allow open access to the domain".

**Warning: this configuration allows anybody to query your database or make requests, so it should not be used in production!**

Click the "Next" button.

#### Finish

Click "Confirm and create" to create your domain.

This process can take several minutes.

#### Get the final domain name

Go back to the Elasticsearch Service dashboard and click on your domain. The "Endpoint" string tells you your Elasticsearch cluster's domain name. This string is the value you should set for the "host" key in the Elasticsearch target section of your EmrEtlRunner [configuration YAML](Common-configuration).

[aes]: https://aws.amazon.com/elasticsearch-service/
