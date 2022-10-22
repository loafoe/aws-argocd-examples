# hsp-aws-argocd-examples

Example ArgoCD projects for deploying workloads to Healthsuite Managed AWS subaccount EKS clusters

## Deployment

You can deploy this project using ArgoCD

## Canary deployment

You can trigger a canary deployment using the below `values.yaml`. Replace the `host` value
with the frontdoor URL you'd like to use in your cluster

```yaml

blue:
  color: "blue"
  ingress:
    enabled: true
    className: "nginx"
    hosts:
    - host: test.us-east-1-xxxx.cluster-id-here.hsp.philips.com
      paths:
        - path: /
          pathType: ImplementationSpecific

green:
   color: "green"
   ingress:
     annotations:
       nginx.ingress.kubernetes.io/canary: "true"
       nginx.ingress.kubernetes.io/canary-weight: "50"
     enabled: true
     className: "nginx"
     hosts:
     - host: test.us-east-1-xxxx.cluster-id-here.hsp.philips.com
       paths:
        - path: /
          pathType: ImplementationSpecific
```
