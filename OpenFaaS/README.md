### Reference

https://www.shogan.co.uk/kubernetes/raspberry-pi-kubernetes-cluster-with-openfaas-for-serverless-functions-part-4

### Exercise simple functions with the CLI

```s.hendrickson@BLD-ML-00027935 raspberry-pi-k8s-experiments % faas-cli invoke nodeinfo  
Reading from STDIN - hit (Control + D) to stop.
Hostname: nodeinfo-5f989fb4cb-r8j5n

Arch: arm64
CPUs: 4
Total mem: 7807MB
Platform: linux
Uptime: 437384.85
```

```
s.hendrickson@BLD-ML-00027935 raspberry-pi-k8s-experiments % faas-cli describe nodeinfo
Name:               nodeinfo
Status:             Ready
Replicas:           1
Available Replicas: 1
Invocations:        14
Image:              ghcr.io/openfaas/nodeinfo:latest
Function Process:   <default>
URL:                http://127.0.0.1:8080/function/nodeinfo
Async URL:          http://127.0.0.1:8080/async-function/nodeinfo
Labels:
 faas_function: nodeinfo
Annotations:
 prometheus.io.scrape: false
 ```

### Directly Through the Gateway

```
s.hendrickson@BLD-ML-00027935 raspberry-pi-k8s-experiments % curl -X POST -d"The cat in the hat came back" http://192.168.127.7/function/sentimentanalysis
{"polarity": 0.0, "subjectivity": 0.0, "sentence_count": 1}
```

### Creating new Function
```angular2html
s.hendrickson@BLD-ML-00027935 OpenFaaS % faas template pull https://github.com/openfaas-incubator/python-flask-template
Fetch templates from repository: https://github.com/openfaas-incubator/python-flask-template at 
2022/08/31 14:18:33 Attempting to expand templates from https://github.com/openfaas-incubator/python-flask-template
2022/08/31 14:18:34 Fetched 7 template(s) : [python27-flask python3-flask python3-flask-armhf python3-flask-debian python3-http python3-http-armhf python3-http-debian] from https://github.com/openfaas-incubator/python-flask-template
```

```angular2html
faas-cli new --lang python3-http-armhf my-first-faas-function
Folder: my-first-faas-function created.
  ___                   _____           ____
 / _ \ _ __   ___ _ __ |  ___|_ _  __ _/ ___|
| | | | '_ \ / _ \ '_ \| |_ / _` |/ _` \___ \
| |_| | |_) |  __/ | | |  _| (_| | (_| |___) |
 \___/| .__/ \___|_| |_|_|  \__,_|\__,_|____/
      |_|


Function created in folder: my-first-faas-function
Stack file written: my-first-faas-function.yml
```
Edit top of python3-http-armhf template docker file to point to a modern base python
image...

```
FROM ghcr.io/openfaas/of-watchdog:0.9.6 as watchdog
#FROM armhf/python:3.9-alpine
FROM arm64v8/python:3.9-alpine
```

Then,
```yaml
OpenFaaS % faas up -f my-first-faas-function.yml
```


