# argo-cm-repro

This repo serves to help understand and debug how to reference a config map created outside of Argo with Kustomize. 


Create a namespace
```
kubectl create namespace demo
```

Create a ConfigMap 
```
kubectl create configmap example-configmap \
  --from-literal=example_domain=example.com \
  --from-literal=example_arn=arn:aws:iam::123456789012:user/Bob \
  --namespace=demo
```

