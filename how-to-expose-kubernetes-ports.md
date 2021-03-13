# How to expose Kubernetes ports


## kubectl port-forward

Temporary exposing of a port

```powershell
kubectl port-forward svc/rabbitmq 15672:15672
```


## kubectl port-forward

```powershell
kubectl expose pod rabbitmq-0 --port=15672 --target-port=15672 --type=NodePort
```

List all services and get the nodePort which can be used locally, e.g. localhost:{nodePort}

```powershell
kubectl get services
```



## service ingress

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
