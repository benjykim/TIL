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
