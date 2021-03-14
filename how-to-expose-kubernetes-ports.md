# How to expose Kubernetes ports

All urls and ports inside the kubernetes cluster are not accessible per default from outside. There are several ways to expose them so that we can interact with them.

## kubectl port-forward

Kubectl port-forward allows you to access and interact with internal Kubernetes cluster processes from your localhost. You can use this method to investigate issues and adjust your services locally without the need to expose them beforehand.

It will expose the port temporary while the script is running and performs load balancing. Accesible as localhost:15672.


```powershell
kubectl port-forward svc/rabbitmq 15672:15672
```


## kubectl expose pod

Looks up a deployment, service, replica set, replication controller or pod by name and uses the selector for that
resource as the selector for a new service on the specified port. A deployment or replica set will be exposed as a
service only if its selector is convertible to a selector that service supports, i.e. when the selector contains only
the matchLabels component. Note that if no port is specified via --port and the exposed resource has multiple ports, all
will be re-used by the new service. Also if no labels are specified, the new service will re-use the labels from the
resource it exposes.

It will expose the port permanently but without load balancing. Accesible as localhost:15672.


```powershell
kubectl expose pod rabbitmq-0 --port=15672 --target-port=15672 --type=NodePort
```

List all services and get the nodePort which can be used locally, e.g. localhost:{nodePort}

```powershell
kubectl get services
```

## service nodeport

Exposes the Service on each Node's IP at a static port (the NodePort). A ClusterIP Service, to which the NodePort Service routes, is automatically created. You'll be able to contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>

Using a NodePort gives you the freedom to set up your own load balancing solution, to configure environments that are not fully supported by Kubernetes, or even to just expose one or more nodes' IPs directly.

It will expose the port permanently and performs load balancing. Accesible as localhost:30012.

```powershell
kubectl describe pod rabbitmq
```

```yaml
Labels:       app.kubernetes.io/instance=rabbitmq
              app.kubernetes.io/managed-by=Helm
              app.kubernetes.io/name=rabbitmq
              controller-revision-hash=rabbitmq-558574dd9c
              helm.sh/chart=rabbitmq-8.11.3
              statefulset.kubernetes.io/pod-name=rabbitmq-0
```	  

```yaml
apiVersion: v1
kind: Service
metadata:
  name: rabbitmq-nodeport
spec:
  type: NodePort
  selector:
    app.kubernetes.io/instance: rabbitmq # label of the pod
  ports:
    - name: amqp
      protocol: TCP
      port: 5672
      targetPort: 5672
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 30011
    - name: http
      protocol: TCP
      port: 15672
      targetPort: 15672
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 30012
```

save as rabbitmq-nodeport.yaml

```powershell
kubectl apply -f rabbitmq-nodeport.yaml
```


## service ingress

NodePorts let you expose a service by specifying that value in the service’s type. Ingress, on the other hand, is a completely independent resource to your service. You declare, create and destroy it separately to your services.

This makes it decoupled and isolated from the services you want to expose. It also helps you to consolidate routing rules into one place.

It will expose the port permanently, performs load balancing and dns name resolution. Accesible as localhost/rabbitmq.

An Ingress controller is required and must be installed. NGinx is the most popular one.

```bash
helm show values ingress-nginx/ingress-nginx > nginx-deployment.yaml
```

```bash
helm install -f nginx-deployment.yaml nginx ingress-nginx/ingress-nginx 
```

```yaml
  # rabbitmq-ui-ingress.yaml 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
  name: rabbitmq-ui-ingress
spec:
  rules:
    - host:
      http:
        paths:
          - path: /rabbitmq(/|$)(.*)
            pathType: Exact
            backend:
              service:
                name: rabbitmq
                port: 
                  number: 15672
```
The amqp port cannot be used in http proxies like nginx. The solution service nodeport is the best way.


```powershell
kubectl apply -f rabbitmq-ui-ingress.yaml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: rabbitmq-amqp-nodeport
spec:
  type: NodePort
  selector:
    app.kubernetes.io/instance: rabbitmq # label of the pod
  ports:
    - name: amqp
      protocol: TCP
      port: 5672
      targetPort: 5672
      nodePort: 30011
```

save as rabbitmq-amqp-nodeport.yaml

```powershell
kubectl apply -f rabbitmq-amqp-nodeport.yaml
```

