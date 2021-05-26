# KodeKloud-PracticeTest-Deployments
1. How many PODs exist on the system? In the current(default) namespace.
```
$ kubectl get po -n default
```
2. How many ReplicaSets exist on the system? In the current(default) namespace.
```
$ kubectl get rs -n default
```
3. How many Deployments exist on the system? In the current(default) namespace.
```
$ kubectl get deploy -n default
```
4. How many Deployments exist on the system now? We just created a Deployment! Check again!
```
$ kubectl get deploy -n default
```
5. How many ReplicaSets exist on the system now?
```
$ kubectl get rs -n default
```
6. How many PODs exist on the system now?
```
$ kubectl get po -n default
```
7. Out of all the existing PODs, how many are ready?
```
$ kubectl get po -n default
```
8. What is the image used to create the pods in the new deployment?
```
$ kubectl describe pod frontend-deployment-56d8ff5458-fh42q | grep -i image
```
9. Why do you think the deployment is not ready?
```
kubectl describe pod frontend-deployment-56d8ff5458-fh42q
```
10.  Create a new Deployment using the  `deployment-definition-1.yaml`  file located at  `/root/`. There is an issue with the file, so try to fix it.
```
apiVersion: apps/v1
kind: deployment (x) -> Deployment (o)
metadata:
  name: deployment-1
spec:
  replicas: 2
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      labels:
        name: busybox-pod
    spec:
      containers:
      - name: busybox-container
        image: busybox888
        command:
        - sh
        - "-c"
        - echo Hello Kubernetes! && sleep 3600
```
11. Create a new Deployment with the below attributes using your own deployment definition file.
Name:  `httpd-frontend`;  
Replicas:  `3`;  
Image:  `httpd:2.4-alpine`
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      name: httpd-frontend
  template:
    metadata:
      labels:
        name: httpd-frontend
    spec:
      containers:
      - name: httpd-frontend-container
        image: httpd:2.4-alpine
```
