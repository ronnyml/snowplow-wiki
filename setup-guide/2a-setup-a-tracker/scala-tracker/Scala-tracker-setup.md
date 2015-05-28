<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2: setup a Tracker**](Setting-up-a-Tracker) > [**Scala tracker**](Scala-tracker-setup)

## Contents

- 1. [Overview](#overview)  
- 2. [Setup](#setup)

<a name="overview" />
## 1. Overview

The [Snowplow Scala Tracker](https://github.com/snowplow/snowplow-scala-tracker) lets you add analytics to your Scala apps and servers.

Setting up the tracker should be relatively straightforward if you are familiar with [sbt][sbt].

<a name="setup" />
## 2. Setup

Add the Scala Tracker to your build.sbt like this:

```scala
libraryDependencies += "com.snowplowanalytics.snowplow" % "snowplow-scala-tracker" % "0.1.0"
```

Done? Now read the [Scala Tracker API](Scala-Tracker) to start tracking events.

[Back to top](#top)

[sbt]: http://www.scala-sbt.org/
