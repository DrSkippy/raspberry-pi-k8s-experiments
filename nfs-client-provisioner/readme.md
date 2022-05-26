# NFS client provisioner for Microk8s cluster

From:
    https://opensource.com/article/20/6/kubernetes-nfs-client-provisioning

0. Shares
```angular2html
ubuntu@k8s-worker-04:/mnt/disk/vol1$ sudo cat /etc/exports 
# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/mnt/disk/vol1/scratch	192.168.127.0/24(rw,sync)
/mnt/disk/vol1	192.168.127.0/24(rw,sync)
/mnt/disk/vol1/k8s_nfs	192.168.127.0/24(rw,sync)
```

```angular2html
ubuntu@k8s-worker-04:/mnt/disk/vol1$ chmod o+w k8s_nfs/
ubuntu@k8s-worker-04:/mnt/disk/vol1$ ll
total 40
drwxr-xr-x  7 ubuntu ubuntu  4096 May 25 03:42 .
drwxr-xr-x  3 root   root    4096 May 24 03:44 ..
drwxrwxr-x  6 ubuntu ubuntu  4096 Mar  1 21:47 Working
drwx--x--x 14 root   root    4096 May 26 18:34 docker
drwxrwxrwx  2 ubuntu ubuntu  4096 May 26 18:16 k8s_nfs
drwx------  2 ubuntu ubuntu 16384 Jan  5 21:51 lost+found
drwxrwxr-x  2 ubuntu ubuntu  4096 May 25 03:42 scratch
```


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

```
How do we know it works?



before test pod:
```
ubuntu@k8s-worker-04:/mnt/disk/vol1$ ls k8s_nfs/
default-test-claim-pvc-456464a4-c78c-4434-87d3-71614817e27c
ubuntu@k8s-worker-04:/mnt/disk/vol1$ ls k8s_nfs/default-test-claim-pvc-456464a4-c78c-4434-87d3-71614817e27c/
.  ..

kubectl apply -f ./tmp/Working/raspberry-pi-k8s-experiments/nfs-client-provisioner/test-pod.yaml
```

after test pod:
```
ubuntu@k8s-worker-04:/mnt/disk/vol1$ ls -a k8s_nfs/default-test-claim-pvc-456464a4-c78c-4434-87d3-71614817e27c/
.  ..  SUCCESS

```