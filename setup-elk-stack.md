# Set up the ELK Stack - ElasticSearch, Logstash and Kibana


## Install ElasticSearch

Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data for lightning fast search, fine‑tuned relevancy, and powerful analytics that scale with ease.

```bash
curl -O https://raw.githubusercontent.com/elastic/Helm-charts/master/elasticsearch/examples/minikube/values.yaml
```

The values are useful to reduce the hardware resources usage
```bash
helm install elasticsearch elastic/elasticsearch -f ./values.yaml 
```
## Install Kibana

Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack. Do anything from tracking query load to understanding the way requests flow through your apps.

```bash
helm install kibana elastic/kibana
```

## Install Logstash

Logstash is a free and open server-side data processing pipeline that ingests data from a multitude of sources, transforms it, and then sends it to your favorite "stash."

```bash
helm install logstash elastic/logstash
```

## Optional: Install APM Server

Elastic APM is a free and open application performance monitoring system built on the Elastic Stack. This component, APM Server, validates and processes events from APM agents, transforms the data into Elasticsearch documents, and stores it in corresponding Elasticsearch indices.

```bash
helm install apm-server elastic/apm-server
```

## See also

* [Artifact Hub](https://artifacthub.io/)
 