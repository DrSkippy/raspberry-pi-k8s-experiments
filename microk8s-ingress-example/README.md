# microk8s-ingress-example

Microk8s ingress example

Build dockerfile and push to docker repository (using local repository in my case)
```
docker build . -t my-microk8s-app
docker tag my-microk8s-app localhost:5000/my-microk8s-app
docker push localhost:5000/my-microk8s-app
```


Build and push to microk8s repo from a pi machine:

```angular2html
https://medium.com/manikkothu/build-and-deploy-apps-on-microk8s-1df26d1ddd3c
```

``` 
 1285  sudo vim /etc/docker/daemon.json 
 1286  sudo systemctl restart docker
 1287  curl 10.1.235.207:5000/
 1288  curl 10.1.235.207:5000/v2
 1289  docker tag my-microk8s-app 10.1.235.207:5000/my-microk8s-app
 1290  docker push 10.1.235.207:5000/my-microk8s-app
 1291  history
ubuntu@k8s-worker-01:/mnt/disk/vol1/working/raspberry-pi-k8s-experiments/microk8s-ingress-example$  curl 10.1.235.207:5000/v2
<a href="/v2/">Moved Permanently</a>.

ubuntu@k8s-worker-01:/mnt/disk/vol1/working/raspberry-pi-k8s-experiments/microk8s-ingress-example$ curl 10.1.235.207:5000/v2/_catalog
{"repositories":["my-microk8s-app"]}
```


```
ubuntu@k8s-worker-01:/mnt/disk/vol1/working/raspberry-pi-k8s-experiments/microk8s-ingress-example$ cat /etc/docker/daemon.json 
 {
   "exec-opts": ["native.cgroupdriver=systemd"],
   "log-driver": "json-file",
   "log-opts": {
     "max-size": "100m"
   },
   "insecure-registries" : ["10.1.235.207:5000"],
   "storage-driver": "overlay2"
 }
```

```
6. Run Applications And Ingress

microk8s.kubectl apply -f bar-deployment.yml
microk8s.kubectl apply -f foo-deployment.yml
microk8s.kubectl apply -f ingress.yml


If you skip this step you'll get a 503 service unavailable

microk8s.kubectl expose deployment foo-app --type=LoadBalancer --port=8080
microk8s.kubectl expose deployment bar-app --type=LoadBalancer --port=8080

8. Testing Endpoint Out

curl -kL https://127.0.0.1/bar
curl -kL https://127.0.0.1/foo
```

```
s.hendrickson@BLD-ML-00027935 ~ % curl -kL https://192.168.127.7/foo                                        
Hello World from App FOO%                                                                                                                                                     s.hendrickson@BLD-ML-00027935 ~ % curl -kL https://192.168.127.7/bar
Hello World from App BAR%    
```
