###### On the master server run:
`sudo kubeadm certs renew all`

###### To confirm all is well run:
 `kubectl get pods`
 ###### Incase you get the error: 
error: You must be logged in to the server (Unauthorized)

###### Copy file client-key and client certificare data from /etc/kubernetes/admin.conf to ./kube/config
