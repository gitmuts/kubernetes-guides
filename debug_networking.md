We have a cluster, that we are able to create pods successfully, but they are unable to serve external traffic.
Both curl the cluster ip and external ip fails, we are on a discovery journey to understand why it's failing
  
Areas to check (investigating their roles)
1. kubeproxy
2. weavenet
3. iptables


##### 1. Kube proxy
it configures iptables?
kube proxy has the modes:
1) user space proxy mode
  >opens a port randomly for each new service on the node 
3) iptables mode 
  >it creates an iptable rule for each new service, using the iptable rule, routes traffic to the pods(get more on this)
4) IPVS
  >not used this [https://kubernetes.io/docs/concepts/services-networking/service/]

##### 2. weavenet
Create a layer two network using linux kernel features (**Q** i) How is layer two create? ii) which linux features are used?

Weave create a virtual ethernet switch (VETHX)

** container network interface

**check the version of weavenet used : kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"**

What are in this directories:
```
/opt/cni/bin
/etc/cni/net.d/
```

when a pod is started it is joined to the weave network.


##### 3. iptables
check the contents of _iptables nat, mangle, filter_



##### Random concepts

`
Ip's in use are 1) Pod IP 2) Cluster IP (this is when services are defined)
`
