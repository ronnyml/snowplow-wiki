<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 4: setting up alternative data stores**](Setting-up-alternative-data-stores) > [**Setup Druid**](Setup Druid) > Setup Druid single instance

## 1. Overview and warning

This is the **single-instance setup guide** for getting a local version of Druid working with Snowplow. This is designed for development and testing.

**This deployment must NOT be used to put Druid into production with Snowplow - it does NOT scale.**

## 2. Apache ZooKeeper

You will need [Apache ZooKeeper] [apache-zookeeper] running on your machine.

First download it:

```
curl http://www.gtlib.gatech.edu/pub/apache/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz -o zookeeper-3.4.6.tar.gz
sudo tar xzf zookeeper-3.4.6.tar.gz -C /opt
cd /opt/zookeeper-3.4.6
```

Copy across the default ZooKeeper configuration:

```
sudo cp conf/zoo_sample.cfg conf/zoo.cfg
```

Finally start ZooKeeper:

```
sudo ./bin/zkServer.sh start
```

Leave this terminal running.

## 3. Druid server

You will need [Druid] [druid] server running on your machine.

Download the Druid binaries:

```
$ wget http://static.druid.io/artifacts/releases/druid-0.8.3-bin.tar.gz
$ tar -xvf druid-0.8.3-bin.tar.gz -C /opt
LICENSE               extensions-repo       run_example_client.sh
config                lib                   run_example_server.sh
examples              run_druid_server.sh   select_example.sh
```

Now start up Druid:

```
$ xxx
```

Leave this terminal running.

### 4. Tranquility Server

You will need [Tranquility Server] [tranquility-server] running on your machine.

First download it:

```
$ wget http://static.druid.io/tranquility/releases/tranquility-distribution-0.7.2.tgz
$ tar -xvf tranquility-distribution-0.7.2.tgz -C /opt
$ cd ...
```

Now download the Snowplow configuration file for Tranquility Server:

```
$ wget xxx
``` 

Now start up Tranquility Server:

```
$ bin/tranquility server -configFile conf/server.json
```


[apache-zookeeper]: https://zookeeper.apache.org/
[druid]: http://druid.io/
[tranquility-server]: https://github.com/druid-io/tranquility/blob/master/docs/server.md
