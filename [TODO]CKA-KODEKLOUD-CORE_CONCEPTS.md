# KodeKloud-PracticeTest-Pods
1. How many  `pods`  exist on the system? In the current(default) namespace.
```
$ kubectl get pods --namespace default
$ kubectl get pods -n default // "--namespace" == "-n"
```
2.  Create a new pod with the `nginx` image.
```
$ kubectl run nginx --image=nginx
```
3.  How many pods are created now? Note: We have created a few more pods. So please check again.
```
$ kubectl get pods -A
```
4. What is the image used to create the new pods? You must look at one of the new pods in detail to figure this out.
```
$ kubectl describe  pods newpods-jwnxr --namespace default | grep image
```
5. Which nodes are these pods placed on? You must look at all the pods in detail to figure this out.
```
$ kubectl describe pods newpods-jwnxr --namespace default | grep Node
$ kubectl get pods -o wide
```
6. How many containers are part of the pod  `webapp`? Note: We just created a new POD. Ignore the state of the POD for now.
```
$ kubectl describe pods webapp
```
7. What images are used in the new  `webapp`  pod? 
You must look at all the pods in detail to figure this out.
```
$ kubectl describe pods webapp | grep image
```
8. What is the state of the container  `agentx`  in the pod  `webapp`? Wait for it to finish the  `ContainerCreating`  state
```
$ kubectl describe pods webapp
``` 
9. Why do you think the container  `agentx`  in pod  `webapp`  is in error? Try to figure it out from the events section of the pod.
```
$ kubectl describe pods webapp
```
10. What does the READY column in the output of the `kubectl get pods` command indicate?
```
Running Containers in Pods / Total Containers in Pods
```
111. Delete the  `webapp`  Pod. Once deleted, wait for the pod to fully terminate.
```
$ kubectl delete pods webapp
```
12. Create a new pod with the name  `redis`  and with the image  `redis123`. Use a pod-definition YAML file. And yes the image name is wrong!
```
apiVersion: v1
kind: Pod
metadata: 
  name: redis
spec: 
  containers: 
    - name: redis
      image: redis123
```  
13. Now change the image on this pod to  `redis`. Once done, the pod should be in a  `running`  state.
```
$ kubectl edit pods redis
-> change "image: redis123" to "image: redis"
```

---

# KodeKloud-PracticeTest-ReplicaSets
1. How many PODs exist on the system? In the current(default) namespace.
```
$ kubectl get po -n default
```
2. How many ReplicaSets exist on the system? In the current(default) namespace.
```
$ kubectl get replicaset -n default
```
3. How about now? How many ReplicaSets do you see? We just made a few changes!
```
$ kubectl get rs 
```
4. How many PODs are DESIRED in the `new-replica-set`?
```
$ kubectl get rs
```
5. What is the image used to create the pods in the `new-replica-set`?
```
$ kubectl describe rs new-replica-set | grep -i image
// `-i` 옵션 : 대소문자 구분 없이 문자열 찾기
```
6. How many PODs are READY in the `new-replica-set`?
```
$ kubectl get rs new-replica-set
```
7. Why do you think the PODs are not ready?
```
$ kubectl describe po new-replicas-set-9kntt
// po == pods
```
8. Delete any one of the 4 PODs.
```
$ kubectl delete po new-replica-set-9kntt
```
9. How many pods exist?
```
$ kubectl get po
``` 
10. Why are there still 4 PODs, even after you deleted one?
```
ReplicaSet ensures that desired number of PODs always run
```
11. Create a ReplicaSet using the  `replicaset-definition-1.yaml`  file located at  `/root/`. There is an issue with the file, so try to fix it.
```
apiVersion: v1 (x) -> apps/v1 (o)
kind: ReplicaSet
metadata: 
  name: replicaset-1
spec: 
  replicas: 2
  selector: 
    matchLabels: 
      tier: frontend
  template: 
    metadata: 
      labels: 
        tier: frontend
    spec: 
      containers: 
      - name: nginx
        image: nginx
```
12. Fix the issue in the  `replicaset-definition-2.yaml`  file and create a  `ReplicaSet`  using it. This file is located at  `/root/`.
```
apiVersion: apps/v1
kind: ReplicaSet
metadata: 
  name: replicaset-1
spec: 
  replicas: 2
  selector: 
    matchLabels: 
      tier: frontend
  template: 
    metadata: 
      labels: 
        tier: nginx (x) -> frontend (o)
    spec: 
      containers: 
      - name: nginx
        image: nginx
```
13. Delete the two newly created ReplicaSets - `replicaset-1` and `replicaset-2`
```
$ kubectl delete rs replicaset-1 replicaset-2
```
14. Fix the original replica set  `new-replica-set`  to use the correct  `busybox`  image. Either delete and recreate the ReplicaSet or Update the existing ReplicaSet and then delete all PODs, so new ones with the correct image will be created.
```
$ kubectl edit rs new-replica-set
$ kubectl delete pods --all -n default
```
15. Scale the ReplicaSet to 5 PODs. Use  `kubectl scale`  command or edit the replicaset using  `kubectl edit replicaset`.
```
$ kubectl scale --replicas=5 rs/new-replica-set
$ kubectl edit rs new-replica-set
```
16. Now scale the ReplicaSet down to 2 PODs. Use the  `kubectl scale`  command or edit the replicaset using  `kubectl edit replicaset`.
```
$ k scale --current-replicas=5 --replicas=2 rs/new-replica-set
```

---

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
