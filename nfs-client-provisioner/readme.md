# NFS client provisioner for Microk8s cluster

From:
    https://opensource.com/article/20/6/kubernetes-nfs-client-provisioning


1. The original project:
```
git clone https://github.com/kubernetes-incubator/external-storage.git
```
2. Deployent

From worker-01, nfs mounted directory for worker-04
```angular2html
kubectl apply -f ./tmp/Working/raspberry-pi-k8s-experiments/nfs-client-provisioner/rbac.yaml 
kubectl apply -f ./tmp/Working/raspberry-pi-k8s-experiments/nfs-client-provisioner/deployment.yaml 
kubectl apply -f ./tmp/Working/raspberry-pi-k8s-experiments/nfs-client-provisioner/class.yaml
```

results:
```
ubuntu@k8s-worker-01:~$ microk8s.kubectl get deployments
NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
foo-app                  2/2     2            2           111m
bar-app                  2/2     2            2           111m
book-service             2/2     2            2           111m
nfs-client-provisioner   1/1     1            1           6m50s
ubuntu@k8s-worker-01:~$ microk8s.kubectl get storageclass
NAME                          PROVISIONER            RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
microk8s-hostpath (default)   microk8s.io/hostpath   Delete          WaitForFirstConsumer   false                  157m
managed-nfs-storage           rpi-nfs/vol1           Delete          Immediate              false                  5m59s
```

3. Test claim

```angular2html
kubectl apply -f ./tmp/Working/raspberry-pi-k8s-experiments/nfs-client-provisioner/test-claim.yaml
kubectl apply -f ./tmp/Working/raspberry-pi-k8s-experiments/nfs-client-provisioner/test-pod.yaml
```
How do we know it works?