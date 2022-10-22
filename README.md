# hsp-aws-argocd-examples

Example ArgoCD projects for deploying workloads to Healthsuite Managed AWS subaccount EKS clusters

## Deployment

You can deploy this project using ArgoCD. Clone this repository and add it to your ArgoCD reposities list. 
Then create an application using the `charts/blue-green-canary` Chart. The default values will only activate
the `blue` deployment.

## Canary

You can trigger a canary deployment using the below `values.yaml`. Replace the `host` value
with the frontdoor URL you'd like to use in your cluster. In the example the traffic evenly
between the blue and green deployment. Change the `nginx.ingress.kubernetes.io/canary-weight`
to `100` to move all traffic to the green application. You can then update the `blue` version 
with your next release, rinse and repeat.


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

