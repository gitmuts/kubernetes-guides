## Create a directory on the NFS server
```

mkdir /home/appadmin/applogs
chmod 777 /home/appadmin/applogs/

vim /etc/exports
```
## Add the client servers that should access the NFS ie.  the worker nodes

```
/home/appadmin/applogs/ *.*.*.*(no_root_squash,rw,sync)
/home/appadmin/applogs/ *.*.*.*(no_root_squash,rw,sync)
```
Replace *.*.*.* with the actual Ip's.

### To reload nfs configs run:

```
exportfs -r
```

To confirm the configuration run: `exportfs -v`, you should see a list of allowed clients and the directories


## Mount it to the pod

### Create Persistent volume and persistent volume claim config

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: backendlogs-pvc
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 7Gi
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: backendlogs-pv-volume
  labels:
    type: local
spec:
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  capacity:
    storage: 7Gi
  accessModes:
    - ReadWriteMany
  nfs:
    path: /home/appadmin/logs  # Directory created
    server: REPLACE_WITH_NFS_SERVER
    readOnly: false
```

### Add a volume defination under the pod spec:

```
    spec:
      containers:
      ...
      volumes:
        - name: logs-storage
          persistentVolumeClaim:
            claimName: backendlogs-pvc
            
```

### Add config under container configuration

```
      containers:
        - name: sampleapp
          image: IMAGE_URL
          imagePullPolicy: Always
          volumeMounts:
            - name: logs-storage
              mountPath: /logs
```


Run kubectl apply for the changes to pick.

Reference:

https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-setup-nfs-server-on-centos-7-rhel-7-fedora-22.html
