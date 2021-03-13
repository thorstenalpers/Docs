# Set up the ELK Stack - ElasticSearch, Logstash and Kibana


## Install ElasticSearch

Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data for lightning fast search, fine‑tuned relevancy, and powerful analytics that scale with ease.

```bash
curl https://raw.githubusercontent.com/elastic/helm-charts/master/elasticsearch/values.yaml -o elastic-deployment.yaml
```

Reuce the replicas and storage size.
```bash
helm install my-elasticsearch elastic/elasticsearch -f ./elastic-deployment.yaml 
```
## Install Kibana

Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack. Do anything from tracking query load to understanding the way requests flow through your apps.

```bash
curl https://raw.githubusercontent.com/elastic/helm-charts/master/kibana/values.yaml -o kibana-deployment.yaml
```

```bash
helm install kibana elastic/kibana -f kibana-deployment.yaml
```

## Optional: Install Logstash

Logstash is a free and open server-side data processing pipeline that ingests data from a multitude of sources, transforms it, and then sends it to your favorite "stash."

```bash
curl https://raw.githubusercontent.com/elastic/helm-charts/master/logstash/values.yaml -o logstash-deployment.yaml
```

```bash
helm install logstash elastic/logstash -f logstash-deployment.yaml
```

## Optional: Install Filebeat

Whether you’re collecting from security devices, cloud, containers, hosts, or OT, Filebeat helps you keep the simple things simple by offering a lightweight way to forward and centralize logs and files.

```bash
curl https://raw.githubusercontent.com/elastic/helm-charts/master/filebeat/values.yaml -o filebeat-deployment.yaml
```

```bash
helm install filebeat elastic/filebeat -f filebeat-deployment.yaml
```

## Optional: Install APM Server

Elastic APM is a free and open application performance monitoring system built on the Elastic Stack. This component, APM Server, validates and processes events from APM agents, transforms the data into Elasticsearch documents, and stores it in corresponding Elasticsearch indices.

```bash
curl https://raw.githubusercontent.com/elastic/helm-charts/master/apm-server/values.yaml -o apm-deployment.yaml
```

```bash
helm install apm-server elastic/apm-server -f apm-deployment.yaml
```

## Optional: Install MetricBeat

Collect metrics from your systems and services. From CPU to memory, Redis to NGINX, and much more, Metricbeat is a lightweight way to send system and service statistics.

```bash
curl https://raw.githubusercontent.com/elastic/helm-charts/master/metricbeat/values.yaml -o metricbeat-deployment.yaml
```

```bash
helm install metricbeat elastic/metricbeat -f metricbeat-deployment.yaml
```

## See also

* [Artifact Hub](https://artifacthub.io/)
 