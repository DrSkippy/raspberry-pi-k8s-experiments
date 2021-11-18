# microk8s-ingress-example

Microk8s ingress example
Following very closely:

https://kndrck.co/posts/microk8s_ingress_example/

Build dockerfile and push to docker repository
* 2021 Nov 18
* I did this on worker-02
* Build an image for arm

```
docker build . -t my-microk8s-app
docker tag my-microk8s-app localhost:5000/my-microk8s-app
docker push localhost:5000/my-microk8s-app
```

Examples and explanations for build and push to microk8s repo from a pi machine:

https://medium.com/manikkothu/build-and-deploy-apps-on-microk8s-1df26d1ddd3c

From commandline on worker-01
* cluster repo is on master and at port 5000
* configure docker for insecure repo push buy editing config and restarting

``` 
sudo vim /etc/docker/daemon.json    # edit to ip shown below, for example
sudo systemctl restart docker
docker tag my-microk8s-app 10.1.235.207:5000/my-microk8s-app
docker push 10.1.235.207:5000/my-microk8s-app

ubuntu@k8s-worker-01:/mnt/disk/vol1/working/raspberry-pi-k8s-experiments/microk8s-ingress-example$ curl 10.1.235.207:5000/v2/_catalog
{"repositories":["my-microk8s-app"]}
```

This was my edit of /etc/docker/daemon.json:

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


Run Applications And Ingress
* major edits to ingress.yml to meet now data scecs

```
microk8s.kubectl apply -f bar-deployment.yml
microk8s.kubectl apply -f foo-deployment.yml
microk8s.kubectl apply -f ingress.yml
```

If you skip this step you'll get a 503 service unavailable
```
microk8s.kubectl expose deployment foo-app --type=LoadBalancer --port=8080
microk8s.kubectl expose deployment bar-app --type=LoadBalancer --port=8080
```
Testing Endpoint Out

```
curl -kL https://127.0.0.1/bar
curl -kL https://127.0.0.1/foo
```

On OSX, test it out:

```
s.hendrickson@BLD-ML-00027935 ~ % curl -kL https://192.168.127.7/foo                                        
Hello World from App FOO%                                                                                                                                           s.hendrickson@BLD-ML-00027935 ~ % curl -kL https://192.168.127.7/bar
Hello World from App BAR%    
```
