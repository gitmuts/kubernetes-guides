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


Reference:

https://www.itzgeek.com/how-tos/linux/centos-how-tos/how-to-setup-nfs-server-on-centos-7-rhel-7-fedora-22.html
