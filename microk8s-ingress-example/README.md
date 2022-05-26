# microk8s-ingress-example

Microk8s ingress example, following very closely:

    https://kndrck.co/posts/microk8s_ingress_example/

See Evernote for registry setup.

Build dockerfile and push to docker repository
* 2021 Nov 18 - I did this on worker-02
* 2022 May 25 - I did thi son worker-04 (USB mount)
* Build an image for arm

```
docker build . -t my-microk8s-app
docker tag my-microk8s-app localhost:32000/my-microk8s-app
docker push localhost:32000/my-microk8s-app
```

Examples and explanations for build and push to microk8s repo from a pi machine:

    https://medium.com/manikkothu/build-and-deploy-apps-on-microk8s-1df26d1ddd3c

See what is in the docker registry:
```angular2html
docker images ls
docker container ls
```
Or in the microk8s registry:
```
curl localhost:3200/v2/_catalog
curl localhost:32000/v2/book-service/tags/list
```

Deploy Applications And Ingress
* major edits to ingress.yml to meet new data specs
* ingress includes the book db API - One ingress def per cluster, not by app, pod, etc.

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
curl -kL https://192.168.127.11/foo                                        
curl -kL https://192.168.127.10/bar
```

In a browser, try:
```angular2html
http://192.168.127.11/foo
http://192.168.127.11/bar
```
