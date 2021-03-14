# Set up the ELK Stack - ElasticSearch, Logstash and Kibana


## Install ElasticSearch

Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data for lightning fast search, fine‑tuned relevancy, and powerful analytics that scale with ease.

```bash
helm show values elastic/elasticsearch > elasticsearch-deployment.yaml
```

Reuce the replicas and storage size.
```bash
helm install elasticsearch elastic/elasticsearch -f ./elasticsearch-deployment.yaml 
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: elasticsearch-nodeport
spec:
  type: NodePort
  selector:
    app.kubernetes.io/instance: elasticsearch # label of the pod
  ports:
    - name: elastic
      protocol: TCP
      port: 5672
      targetPort: 5672
      nodePort: 30011
```

save as elasticsearch-nodeport.yaml

```powershell
kubectl apply -f elasticsearch-nodeport.yaml
```
## Install Kibana

Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack. Do anything from tracking query load to understanding the way requests flow through your apps.

```bash
helm show values elastic/kibana > kibana-deployment.yaml
```

```bash
helm install kibana elastic/kibana -f kibana-deployment.yaml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: kibana-nodeport
spec:
  type: NodePort
  selector:
    app: kibana
  ports:
    - name: http
      protocol: TCP
      port: 5601
      targetPort: 5601
      nodePort: 30021
```

save as kibana-nodeport.yaml

```powershell
kubectl apply -f kibana-nodeport.yaml
```

Enter http://localhost:30021/ to access kibana.

## Optional: Install Logstash

Logstash is a free and open server-side data processing pipeline that ingests data from a multitude of sources, transforms it, and then sends it to your favorite "stash."

```bash
helm show values elastic/logstash > logstash-deployment.yaml
```

```bash
helm install logstash elastic/logstash -f logstash-deployment.yaml
```

## Optional: Install Filebeat

Whether you’re collecting from security devices, cloud, containers, hosts, or OT, Filebeat helps you keep the simple things simple by offering a lightweight way to forward and centralize logs and files.

```bash
helm show values elastic/filebeat > filebeat-deployment.yaml
```

```bash
helm install filebeat elastic/filebeat -f filebeat-deployment.yaml
```

## Optional: Install APM Server

Elastic APM is a free and open application performance monitoring system built on the Elastic Stack. This component, APM Server, validates and processes events from APM agents, transforms the data into Elasticsearch documents, and stores it in corresponding Elasticsearch indices.

```bash
helm show values elastic/apm-server > apm-deployment.yaml
```

```bash
helm install apm-server elastic/apm-server -f apm-deployment.yaml
```

## Optional: Install MetricBeat

Collect metrics from your systems and services. From CPU to memory, Redis to NGINX, and much more, Metricbeat is a lightweight way to send system and service statistics.

```bash
helm show values elastic/metricbeat > metricbeat-deployment.yaml
```

```bash
helm install metricbeat elastic/metricbeat -f metricbeat-deployment.yaml
```

## See also

* [Artifact Hub](https://artifacthub.io/)
 