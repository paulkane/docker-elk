# Fluentbit + ELK in minikube

## minikube
https://minikube.sigs.k8s.io/docs/start/

```
minikube start
```

linking docker
```
eval $(minikube docker-env)
```

## Elasticsearch

### deploy
```
kubectl apply -f elasticsearch/kubernetes/01_elasticsearch-svc.yml
kubectl apply -f elasticsearch/kubernetes/02_elasticsearch-configmap.yaml
kubectl apply -f elasticsearch/kubernetes/03_elasticsearch-deployment.yml
```
### logs
```
kubectl logs -f -l name=elasticsearch
```

## Fluentbit

### deploy
```
kubectl apply -f fluentbit/kubernetes/01_fluent-bit-service-account.yaml
kubectl apply -f fluentbit/kubernetes/02_fluent-bit-role.yaml
kubectl apply -f fluentbit/kubernetes/03_fluent-bit-role-binding.yaml
kubectl apply -f fluentbit/kubernetes/04_fluent-bit-configmap.yaml
kubectl apply -f fluentbit/kubernetes/05_fluent-bit-ds-minikube.yaml 
```

if you want to test different Fluentbit filters and parsers edit the file ` fluentbit/kubernetes/04_fluent-bit-configmap.yaml`
you will need to restart the config map and the daemon set
```
kubectl delete -f fluentbit/kubernetes/04_fluent-bit-configmap.yaml
kubectl apply -f fluentbit/kubernetes/04_fluent-bit-configmap.yaml
kubectl delete -f fluentbit/kubernetes/05_fluent-bit-ds-minikube.yaml 
kubectl apply -f fluentbit/kubernetes/05_fluent-bit-ds-minikube.yaml 
```

### logs
```
kubectl logs -f -l name=fluent-bit
```

## logstash
### deploy
```
kubectl apply -f logstash/kubernetes/01_logstash-svc.yaml
kubectl apply -f logstash/kubernetes/02_logstash-configmap.yaml
kubectl apply -f logstash/kubernetes/03_logstash-deployment.yml
```

## kibana
### deploy
```
kubectl apply -f kibana/kubernetes/01_kibana-svc.yaml
kubectl apply -f kibana/kubernetes/02_kibana-configmap.yaml
kubectl apply -f kibana/kubernetes/03_kibana-deployment.yaml
```

### first time kibana
Port forward
```
kubectl port-forward deployment/kibana 5601
```
the window must remain active

You can launch kibana http://localhost:5601/app/home

you will need to create an index pattern: http://localhost:5601/app/management/kibana/indexPatterns

## Testing logfiles

I recommend you most a host folder to minikube; this window needs to remain open and active.
```
minikube mount [hostFolder]:[minikubeFolder]
```
for example:
```
minikube mount ~/logExamples:/var/log/logExamples
```

ssh onto minikube
```
minikube ssh
cd /var/log
sudo su
```
you can either copy your log file to the `containers` folder
```
cp logExamples/myLog.log containers
```

or if you want the kubernetes metadata to still be in your logs folder
```
cat logExamples/myLog.log >> containers/[file.log]
```

## shutdown

```
kubectl delete deploy -l service=logging
kubectl delete ds -l service=logging
```
