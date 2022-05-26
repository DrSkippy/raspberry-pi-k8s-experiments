# NFS Registry Setup
Resources:

```
https://discuss.kubernetes.io/t/how-to-use-the-built-in-registry/11274/4
https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/
```

Enable Microk8s Registry
```
microk8s enable dns
microk8s enable registry
```

Insecure registry access
`````` 
sudo vim /etc/docker/daemon.json    # edit to ip shown below, for example
ubuntu@k8s-worker-01:/mnt/disk/vol1/working/raspberry-pi-k8s-experiments/microk8s-ingress-example$ cat /etc/docker/daemon.json 
 {
   "insecure-registries" : ["localhost:32000"],
 }
sudo systemctl restart docker
``````

Test the registry
```
curl localhost:32000/v2/_catalog
curl localhost:32000/v2/book-service/tags/list
```


Assuming the NFS setup part of this is complete.
```
microk8s.kubectl get persistentvolume
microk8s.kubectl get persistentvolumeclaims --namespace container-registry
```

Setup:
```
mkdir /mnt/disk/vol1/registry
kubectl apply -f registry-pv.yaml 
kubectl get pv
kubectl get pvc --namespace container-registry
kubectl get pvc --namespace container-registry -o yaml
```