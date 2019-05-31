# ELK
How to use Elasticsearch, Logstash and Kibana to visualise logs in Python in realtime



### ELK stack

E — [**Elasticsearch**](https://www.elastic.co/products/elasticsearch), L — [**Logstash**](https://www.elastic.co/products/logstash)**,** K — [**Kibana**](https://www.elastic.co/products/kibana)

Let me give a brief introduction to it. The ELK stack is a collection of three open source softwares that helps in providing realtime insights about data that can be either structured or unstructured. One can search and analyse data using its tools with extreme ease and efficiently.

[**Elasticsearch**](https://www.elastic.co/products/elasticsearch) is a distributed, RESTful search and analytics engine capable of solving a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data so you can discover the expected and uncover the unexpected. Elasticsearch lets you perform and combine many types of searches — structured, unstructured, geo, metric etc. It is built on Java programming language, which enables Elasticsearch to run on different platforms. It enables users to explore very large amount of data at very high speed.

[**Logstash**](https://www.elastic.co/products/logstash) is an open source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to your favourite “stash” (like Elasticsearch). Data is often scattered or siloed across many systems in many formats. Logstash supports a variety of inputs that pull in events from a multitude of common sources, all at the same time. Easily ingest from your logs, metrics, web applications, data stores, and various AWS services, all in continuous, streaming fashion. Logstash has a pluggable framework featuring over 200 plugins. Mix, match, and orchestrate different inputs, filters, and outputs to work in pipeline harmony.

[**Kibana**](https://www.elastic.co/products/kibana) is an open source analytics and visualisation platform designed to work with Elasticsearch. You use Kibana to search, view, and interact with data stored in Elasticsearch indices. You can easily perform advanced data analysis and visualise your data in a variety of charts, tables, and maps. Kibana makes it easy to understand large volumes of data. Its simple, browser-based interface enables you to quickly create and share dynamic dashboards that display changes to Elasticsearch queries in real time.

To get a better picture of the workflow of how the three softwares interact with each other, refer to the following diagram:

![](./images/1_OHP01Lidop3GQZbnwg9s4Q.jpeg))

### Implementation

#### Logging in Python

Here, I chose to explain the implementation of logging in Python because it is the most used language for projects involving communication between multiple machines and internet of things. It’ll help give you an overall idea of how it works.

Python provides a logging system as a part of its standard library, so you can quickly add logging to your application.

```
import logging
```

In Python, logging can be done at 5 different levels that each respectively indicate the type of event. There are as follows:

- **Info** — Designates informational messages that highlight the progress of the application at coarse-grained level.
- **Debug** — Designates fine-grained informational events that are most useful to debug an application.
- **Warning** — Designates potentially harmful situations.
- **Error** — Designates error events that might still allow the application to continue running.
- **Critical** — Designates very severe error events that will presumably lead the application to abort.

Therefore depending on the problem that needs to be logged, we use the defined level accordingly.

> **Note**: Info and Debug do not get logged by default as logs of only level Warning and above are logged.

Now to give an example and create a set of log statements to visualise, I have created a Python script that logs statements of specific format and a message.

Here, the log statements will append to a file named *logFile.txt* in the specified format. I ran the script for three days at different time intervals creating a file containing logs at random like below:

#### Setting up Elasticsearch, Logstash and Kibana

```bash
# Elasticsearch
sudo HTTP_PROXY=http://127.0.0.1:1080 docker pull docker.elastic.co/elasticsearch/elasticsearch:7.1.1

docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.1.1

curl http://localhost:9200/
```



```bash
# Kibana
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.1.1-amd64.deb
sudo dpkg -i kibana-7.1.1-amd64.deb
whereis kibana
sudo vi /etc/kibana/kibana.yml
# add elasticsearch.hosts: ["http://localhost:9200"] to config/kibana.yml
sudo /usr/share/kibana/bin/kibana
curl http://localhost:5601 # management page open at browser
```



```bash
 # Logstash
wget https://artifacts.elastic.co/downloads/logstash/logstash-7.1.1.tar.gz
tar -xf logstash-7.1.1.tar.gz -C ~/part2/opt 
touch config/logstash.conf
bin/logstash -f config/logstash.conf --config.reload.automatic
```

> about configuring logstash  click [here](https://www.elastic.co/guide/en/logstash/current/configuration.html)
>
> auto-load click [here](https://www.elastic.co/guide/en/logstash/current/reloading-config.html#_how_automatic_config_reloading_works)

```bash
# display all the indexes,  a new Index pattern has to be selected from dev tools, after which various visualisation techniques are used to create a dashboard.
curl http://localhost:9200/_cat/indices?v
```



## reference 

[introduction](<https://www.freecodecamp.org/news/how-to-use-elasticsearch-logstash-and-kibana-to-visualise-logs-in-python-in-realtime-acaab281c9de/>)

[learning](<https://www.elastic.co/learn>)

[download elasticsearch](<https://www.elastic.co/downloads/elasticsearch>)

[install elasticsearch from docker](<https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>)

[download Logstash](<https://www.elastic.co/downloads/logstash>)

[download Kibana](<https://www.elastic.co/downloads/kibana>)