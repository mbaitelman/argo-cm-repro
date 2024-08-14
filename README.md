# argo-cm-repro

This repo serves to help understand and debug how to reference a config map created outside of Argo with Kustomize. 


Create a namespace
```
kubectl create namespace demo
```

Create a ConfigMap 
```shell
kubectl create configmap example-configmap \
  --from-literal=example_domain=example.com \
  --from-literal=example_arn=arn:aws:iam::123456789012:user/Bob \
  --namespace=demo
```

Add this repo to ArgoCD
Follow https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/#https-username-and-password-credential

Create a new Application 
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argo-cm-repro
spec:
  destination:
    name: ''
    namespace: 'demo'
    server: 'https://kubernetes.default.svc'
  source:
    path: kustomize-example
    repoURL: 'https://github.com/mbaitelman/argo-cm-repro.git'
    targetRevision: HEAD
  sources: []
  project: default
```

Get error

> Unable to create application: application spec for argo-cm-repro is invalid: InvalidSpecError: Unable to generate manifests in kustomize-example: rpc error: code = Unknown desc = `kustomize build .kustomize-example` failed exit status 1: Error: nothing selected by ConfigMap.[noVer].[noGrp]/example-configmap.demo:data.example_arn