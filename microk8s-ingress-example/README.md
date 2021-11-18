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