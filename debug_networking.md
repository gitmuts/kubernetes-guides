We have a cluster, that we are able to create pods successfully, but they are unable to serve external traffic.
Both curl the cluster ip and external ip fails, we are on a discovery journey to understand why it's failing
  
Areas to check (investigating their roles)
1 kubeproxy
2 weavenet
3 iptables

##### weavenet
Create a layer two network using linux kernel features (**Q** i) How is layer two create? ii) which linux features are used?
