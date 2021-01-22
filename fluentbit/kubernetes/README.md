# Fluent Bit to Elasticsearch on Minikube

```
kubectl create -f 01_fluent-bit-service-account.yaml
kubectl create -f 02_fluent-bit-role.yaml
kubectl create -f 03_fluent-bit-role-binding.yaml
kubectl create -f 04_fluent-bit-configmap.yaml
kubectl create -f 05_fluent-bit-ds-minikube.yaml
```
- source https://docs.fluentbit.io/manual/installation/kubernetes

# Elasticsearch on Minikube
https://logz.io/blog/deploying-the-elk-stack-on-kubernetes-with-helm/


copy logs to minikube
```
minikube mount ~/Projects/paulkane/docker-elk/log_examples:/var/log/paulkane
```