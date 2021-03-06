storm-crawler-elasticsearch
===========================

A collection of resources for [Elasticsearch](https://www.elastic.co/products/elasticsearch):
* [IndexerBolt](https://github.com/DigitalPebble/storm-crawler/blob/master/external/elasticsearch/src/main/java/com/digitalpebble/stormcrawler/elasticsearch/bolt/IndexerBolt.java) for indexing documents crawled with StormCrawler
* [Spouts](https://github.com/DigitalPebble/storm-crawler/blob/master/external/elasticsearch/src/main/java/com/digitalpebble/stormcrawler/elasticsearch/persistence/AggregationSpout.java) and [StatusUpdaterBolt](https://github.com/DigitalPebble/storm-crawler/blob/master/external/elasticsearch/src/main/java/com/digitalpebble/stormcrawler/elasticsearch/persistence/StatusUpdaterBolt.java) for persisting URL information in recursive crawls
* [MetricsConsumer](https://github.com/DigitalPebble/storm-crawler/blob/master/external/elasticsearch/src/main/java/com/digitalpebble/stormcrawler/elasticsearch/metrics/MetricsConsumer.java)
* [StatusMetricsBolt](https://github.com/DigitalPebble/storm-crawler/blob/master/external/elasticsearch/src/main/java/com/digitalpebble/stormcrawler/elasticsearch/metrics/StatusMetricsBolt.java) for sending the breakdown of URLs per status as metrics and display its evolution over time.

as well as an archetype containing a basic crawl topology and its configuration.

We also have resources for [Kibana](https://www.elastic.co/products/kibana) to build basic real-time monitoring dashboards for the crawls, such as the one below.

![bla](https://pbs.twimg.com/media/CR1-waVWEAAh0u4.png)

A dashboard for [Grafana](http://grafana.com/) is available from https://grafana.com/dashboards/2363.

Getting started
---------------------

Use the archetype for Elasticsearch with `mvn archetype:generate -DarchetypeGroupId=com.digitalpebble.stormcrawler -DarchetypeArtifactId=storm-crawler-elasticsearch-archetype`.

You'll be asked to enter a groupId (e.g. com.mycompany.crawler), an artefactId (e.g. stormcrawler), a version and package name.

This will not only create a fully formed project containing a POM with the dependency above but also a set of resources, configuration files and a topology class. Enter the directory you just created (should be the same as the artefactId you specified earlier) and follow the instructions on the README file.

Video tutorial
---------------------

[![Video tutorial](https://i.ytimg.com/vi/KTerugU12TY/hqdefault.jpg)](https://www.youtube.com/watch?v=KTerugU12TY)


Kibana
---------------------

In [Kibana](http://localhost:5601/#/settings/objects),

1. import the dashboard definitions via [Management > Kibana > Save Objects > Import](http://localhost:5601/app/kibana#/management/kibana/objects) and select the file `kibana/status.json`.  Then go to `Dashboards` and click on `Crawl Status`. You should see a table containing a single line _DISCOVERED 1_.
2. repeat the operation with the file `kibana/metrics.json`.

The [Metrics dashboard](http://localhost:5601/#/dashboard/Crawl-metrics) in Kibana can be used to monitor the progress of the crawl.

#### Per time period metric indices (optional)

The _metrics_ index can be configured per tine period. This best practice is [discussed on the Elastic website](https://www.elastic.co/guide/en/elasticsearch/guide/current/time-based.html).

The crawler config YAML must be updated to use an optional argument as shown below to have one index per day:

```
 #Metrics consumers:
    topology.metrics.consumer.register:
         - class: "com.digitalpebble.stormcrawler.elasticsearch.metrics.MetricsConsumer"
           parallelism.hint: 1
           argument: "yyyy-MM-dd"
```








