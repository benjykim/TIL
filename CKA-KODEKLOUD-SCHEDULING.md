# Scheduling 

### Practice Test Manual Scheduling

1. A pod definition file `nginx.yaml` is given. Create a pod using the file. Only create the POD for now. We will inspect its status next.

```bash
$ kubectl apply -f nginx.yaml
```

2. What is the status of the created POD?

```
Pending
```

3. Why is the POD in a pending state? Inspect the environment for various kubernetes control plane components.

```
$ kubectl get pods -A
No Scheduler Present
```

4. Manually schedule the pod on `node01`. Delete and recreate the POD if necessary.

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01 # 새로 추가된 부분
```

5. Now schedule the same pod on the `master/controlplane` node. Delete and recreate the POD if necessary.

```bash
$ kubectl delete po nginx

# change 'nodeName: node01' -> 'nodeName: controlplane'
$ kubectl apply -f nginx.yaml
```

---

### Labels and Selectors

1. We have deployed a number of PODs. They are labelled with `tier`, `env` and `bu`. How many PODs exist in the `dev` environment? Use selectors to filter the output

```bash
$ kubetl get po --selector env=dev
```

2. How many PODs are in the `finance` business unit (`bu`)?

```bash
$ kubetl get po --selector bu=finance
```

3. How many objects are in the `prod` environment including PODs, ReplicaSets and any other objects?

```bash
$ kubectl get po --selector env=prod
$ kubectl get svc --selector env=prod
$ kubectl get rs --selector env=prod
```

4. Identify the POD which is part of the `prod` environment, the `finance` BU and of `frontend` tier?

```bash
$ kubectl get po --selector env=prod,bu=finance,tier=frontend
```

5. A ReplicaSet definition file is given `replicaset-definition-1.yaml`. Try to create the replicaset. There is an issue with the file. Try to fix it.

```yaml
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
        tier: frontend // this attribute is changed
    spec: 
      containers: 
      - name: nginx
        image: nginx
```

---

### Taints and Toleration

1. How many Nodes exist on the system? including the master/controlplane node

```bash
$ kubectl get nodes -A
```

2. Do any taints exist on node01?

```bash
$ kubectl describe nodes node01 | grep Taints
```

3. Create a taint on node01 with key of `spray`, value of `mortein` and effect of `NoSchedule`

```bash
$ kubectl taint nodes node01 spray=mortein:NoSchedule
```

4. Create a new pod with the NGINX image, and Pod name as `mosquito`

```bash
# commands
$ kubectl run mosquito --image nginx

# yaml --> kubectl create -f mosquito.yaml
apiVersion: v1
kind: Pod
metadata: 
  name: mosquito
spec:
  containers: 
  - image: nginx
    name: mosquito
```

5. What is the state of the POD?

```bash
$ kubectl get po -A
```

6. Why do you think the pod is in a pending state?

```bash
# taint 때문에 pending state에 걸려있음
$ kubectl describe po mosquito -n default
```

7. Create another pod named `bee` with the NGINX image, which has a toleration set to the taint `Mortein`

```yaml
apiVersion: v1
kind: Pod
metadata: 
  name: bee
spec: 
  containers:
  - image: nginx
    name: nginx
  tolerations: 
  - key: "spray"
    operator: "Equal"
    value: "mortein"
    effect: "NoSchedule"
```

8. Notice the `bee` pod was scheduled on node `node01` despite the taint.
9. Do you see any taints on `master/controlplane` node?

```bash
# Yes
$ kubectl describe nodes controlplane | grep Taint
```

10. Remove the taint on master/controlplane, which currently has the taint effect of NoSchedule

```bash
$ kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-
```

11. What is the state of the pod `mosquito` now?

```bash
$ kubectl get po -A# Running
```

12. Which node is the POD `mosquito` on now?

```bash
$ kubectl get po -A -o wide# master/controlplane
```

---

### Node Affinity

1. How many Labels exist on node node01?

```bash
# [주의] 'grep Labels' 실행 시 모든 레이블이 나오지 않음
$ kubectl describe nodes node01
$ kubectl get nodes node01 --show-labels
```

2. What is the value set to the label beta.kubernetes.io/arch on node01?

```bash
$ kubectl get nodes node01 --show-labels
```

3. Apply a label `color=blue` to node `node01`

```bash
$ kubectl label nodes node01 color=blue
```

4. Create a new deployment named `blue` with the `nginx` image and 3 replicas

```bash
$ kubectl create deploy blue --image=nginx --replicas=3
```

5. Which nodes `can` the pods for the `blue` deployment placed on? Make sure to check taints on both nodes!

```bash
$ kubectl describe nodes node01 | grep Taint
$ kubectl describe nodes controlplane | grep Taint
```

6. (*)Set Node Affinity to the deployment to place the pods on `node01` only

- Name: blue
- Replicas: 3
- Image: nginx
- NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
- Key: color
- values: blue

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```

7. 새로 생성한 파드들이 어느 노드에 위치해 있는가?

```bash
$ kubectl get po -A -o wide
```

8. Create a new deployment named `red` with the `nginx` image and `2` replicas, and ensure it gets placed on the `master/controlplane` node only.  Use the label - node-role.kubernetes.io/master - set on the master/controlplane node.

- Name: red
- Replicas: 2
- Image: nginx
- NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
- Key: node-role.kubernetes.io/master
- Use the right operator

```bash
# 실행하지 않고 yaml 파일 만들기
$ kubectl create deploy red --image nginx --dry-run -o yaml > mydeploy.yaml
```

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists
```

---

### Resource Limits

1. A pod called `rabbit` is deployed. Identify the CPU requirements set on the Pod in the current(default) namespace

   ```bash
   $ kubectl describe po rabbit -n default
   ```

2. Delete the `rabbit` Pod. Once deleted, wait for the pod to fully terminate.

   ```bash
   $ kubectl delete po rabbit
   ```

3. Another pod called `elephant` has been deployed in the default namespace. It fails to get to a running state. Inspect this pod and identify the `Reason` why it is not running.

   ```bash
   $ kubectl get po -A 
   $ kubectl get describe po elephant
   ```

4. The status `CrashLoopBackOff` indicates that it is failing because the pod is out of memory. Identify the memory limit set on the POD.

5. The `elephant` pod runs a process that consume 15Mi of memory. Increase the limit of the `elephant` pod to 20Mi. Delete and recreate the pod if required. Do not modify anything other than the required fields.

   ```bash
   # get image name
   $ kubectl get describe eleplant
   $ kubectl delete po elephant
   $ kubectl run elephant --image=polinux/stress --limits="memory=20Mi"
   ```

6. Inspect the status of POD. Make sure it's running

7. Delete the `elephant` Pod. Once deleted, wait for the pod to fully terminate.

   ```bash
   $ kubectl delete po elephant
   ```

---

### Daemonsets

1. Get all daemonsets

   ```bash
   $ kubectl get ds -A
   ```

2. Which namespace are the `DaemonSets` created in?

   ```bash
   $ kubectl get ds -A
   ```

3. Which of the below is a `DaemonSet`?

   ```bash
   $ kubectl get ds -A
   ```

4. On how many nodes are the pods scheduled by the **DaemonSet** `kube-proxy`

   ```bash
   $ kubectl describe ds kube-proxy --namespace=kube-system
   ```

5. What is the image used by the POD deployed by the `kube-flannel-ds` **DaemonSet**?

   ```bash
   $ kubectl describe ds kube-flannel-ds -n kube-system
   ```

6. (*)Deploy a **DaemonSet** for `FluentD` Logging. Use the given specifications.

- Name: elasticsearch

- Namespace: kube-system

- Image: k8s.gcr.io/fluentd-elasticsearch:1.20

  ```yaml
  apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    name: elasticsearch
    namespace: kube-system
    labels:
      k8s-ds: elasticsearch
  spec:
    selector:
      matchLabels:
        name: elasticsearch
    template:
      metadata:
        labels:
          name: elasticsearch
      spec:
        containers:
        - name: elasticsearch
          image: k8s.gcr.io/fluentd-elasticsearch:1.20 
  ```

---

### Static PODs

1. How many static pods exist in this cluster in all namespaces?

   ```bash
   # count pods with 'controlplane' appended in the name 
   $ kubectl get po -A
   ```

2. Which of the below components is NOT deployed as a static pod?

   ```bash
   $ kubectl get po -A
   ```

3. Which of the below components is NOT deployed as a static POD?

   ```bash
   $ kubectl get po -A
   ```

   No 2, 3 are same questions. Just find some pods which are appended 'controlplane' in the pod name

4. On which nodes are the static pods created currently?

   ```bash
   $ kubect describe po kube-scheduler-controlplane -n kube-system | grep Node
   ```

5. What is the path of the directory holding the static pod definition files?

   ```
   /etc/kubernetes/manifests
   ```

6. How many pod definition files are present in the manifests folder? 

   ```bash
   $ ls /etc/kubernetes/manifests
   ```

7. What is the docker image used to deploy the kube-api server as a static pod?

   ```bash
   $ vim /etc/kubernetes/manifests/kube-apiserver.yaml
   ```

8. (*)Create a static pod named `static-busybox` that uses the `busybox` image and the command `sleep 1000`

   ```bash
   $ kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
   ```

9. Edit the image on the static pod to use `busybox:1.28.4`

   Just edit /etc/kubernetes/manifests/static-busybox.yaml file

10. (*)We just created a new static pod named **static-greenbox**. Find it and delete it. This question is a bit tricky. But if you use the knowledge you gained in the previous questions in this lab, you should be able to find the answer to it.

    ```bash
    # 노드 IP 주소 찾기
    $ kubectl get pods -A
    # ssh로 node01 접속하기
    $ ssh node01
    # kubelet 확인 및 구성 파일 path 확인
    $ ps -ef | grep /usr/lib/kubelet
    $ grep -i staticpod /var/lib/kubelet/config.yaml/etc/just-to-mess-with-you
    $ cd /etc/just-to-mess-with-you
    $ rm -rf green.yaml
    ```

---

### Multiple Schedulers

1. What is the name of the POD that deploys the default kubernetes scheduler in this environment?

   ```bash
   $ kube-scheduler-controlplane
   ```

2. What is the image used to deploy the kubernetes scheduler? Inspect the kubernetes scheduler pod and identify the image

   ```bash
   $ cd /etc/kubernetes/manifests/
   # find kube-scheduler file and check spec.image 
   
   # 다른 방법
   $ kubectl describe pod kube-scheduler-controlplane -n kube-system 
   ```

3. Deploy an additional scheduler to the cluster following the given specification. Use the manifest file used by kubeadm tool. Use a different port than the one used by the current one.

   - Namespace: kube-system
   - Name: my-scheduler
   - Status: Running
   - Custom Scheduler Name

   ```yaml
   ---
   apiVersion: v1
   kind: Pod
   metadata:
     labels:
       component: my-scheduler
       tier: control-plane
     name: my-scheduler
     namespace: kube-system
   spec:
     containers:
     - command:
       - kube-scheduler
       - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
       - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
       - --bind-address=127.0.0.1
       - --kubeconfig=/etc/kubernetes/scheduler.conf
       - --leader-elect=false
       - --port=10282
       - --scheduler-name=my-scheduler
       - --secure-port=0
       image: k8s.gcr.io/kube-scheduler:v1.19.0
       imagePullPolicy: IfNotPresent
       livenessProbe:
         failureThreshold: 8
         httpGet:
           host: 127.0.0.1
           path: /healthz
           port: 10282
           scheme: HTTP
         initialDelaySeconds: 10
         periodSeconds: 10
         timeoutSeconds: 15
       name: kube-scheduler
       resources:
         requests:
           cpu: 100m
       startupProbe:
         failureThreshold: 24
         httpGet:
           host: 127.0.0.1
           path: /healthz
           port: 10282
           scheme: HTTP
         initialDelaySeconds: 10
         periodSeconds: 10
         timeoutSeconds: 15
       volumeMounts:
       - mountPath: /etc/kubernetes/scheduler.conf
         name: kubeconfig
         readOnly: true
     hostNetwork: true
     priorityClassName: system-node-critical
     volumes:
     - hostPath:
         path: /etc/kubernetes/scheduler.conf
         type: FileOrCreate
       name: kubeconfig
   status: {}
   ```

4. A POD definition file is given. Use it to create a POD with the new custom scheduler. File is located at `/root/nginx-pod.yaml`

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginx
   spec:
     schedulerName: my-scheduler
     containers:
     - image: nginx
       name: nginx
   ```

