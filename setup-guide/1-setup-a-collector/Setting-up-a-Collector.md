<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-SnowPlow) > [**Step 1: setup a Collector**](Setting-up-a-collector)

[[/images/2-collectors.png]]

The SnowPlow collector receives data from SnowPlow trackers and logs that data to S3 for storage and further processing. Setting up a collector is the first step in the SnowPlow setup process.

1. [Choose a Collector](#choose)
2. [Setup a Collector](#setup)

<a name="choose" />
## 1. Choose a Collector 

There are currently three collectors available:

| **Tracker**                                    | **Description**                                     | **Status**       |
|:-----------------------------------------------|:----------------------------------------------------|:-----------------|
| [Cloudfront Collector] [cloudfront-collector]  | A simple, robust and scalable collector powered by AWS Cloudfront | Production-ready |
| [Clojure Collector] [clojure-collector]        | A Clojure-based collector that enables user tracking across domains. Powered by Amazon Elastic Beanstalk | Beta      |
| [SnowCannon (node.js)] [snowcannon]            | A real-time, node.js based collector that enables user tracking across domains | Beta |

### Are you setting up SnowPlow to track users across a single domain, or multiple domains?

If you are tracking users across a single domain, we recommend setting up the [Cloudfront collector] [cloudfront-collector]. 

If you are tracking users across multiple domains, we recommending setting up the [Clojure collector] [clojure-collector]. This sets `user_id`s server side, so you can reliably track user journeys across multiple domains. (In contrast, the [Cloudfront collector] [cloudfront-collector] sets them client side, so users get assigned different `user_id`s on different domains.)

Like the [Clojure Collector] [clojure-collector], [SnowCannon] [snowcannon] supports user tracking across multiple domains. It also generates logs in real-time. Currently,  however, the log file format is not supported by the [EmrEtlRunner] [emretlrunner], making integrating [SnowCannon] [snowcannon] with downstream data processing modules tricky. We will be addressing this in the next few months.  

<a name="setup" />
## 2. Setup your Collector

1. [Setup the Cloudfront Collector now!] [cloudfront-collector]
2. [Setup the Clojure Collector now!] [clojure-collector]
3. [Setup SnowCannon now!] [snowcannon]

Setup your collector? Then proceed to [step 2: setup a tracker] [tracker-setup].

[Return to the setup guide] [setup-guide].



[cloudfront-collector]: Setting-up-the-Cloudfront-collector
[clojure-collector]: Setting-up-the-Clojure-collector
[snowcannon]: SnowCannon-setup-guide
[setup-guide]: Setting-up-SnowPlow
[tracker-setup]: Setting-up-SnowPlow#wiki-step2
[emretlrunner]: Setting-up-SnowPlow#wiki-step3