## Update core dns
##### Create a file:  coredns-aa-patch.yaml, with the contents
```
 spec:
  template:
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: k8s-app
                    operator: In
                    values:
                    - kube-dns
              topologyKey: "kubernetes.io/hostname"
```             
              
##### run 
`kubectl patch deployment coredns -n kube-system --type merge --patch "$(cat coredns-aa-patch.yaml)"`

