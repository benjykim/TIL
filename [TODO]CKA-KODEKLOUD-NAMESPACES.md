# KodeKloud-PracticeTest-Namespaces
1. How many Namespaces exist on the system?
```
$ kubectl get ns -A
```
2. How many pods exist in the `research` namespace?
```
$ kubectl get po -n research
```
3. Create a POD in the  `finance`  namespace. Use the spec given below.
* Name: redis
* Image Name: redis
```
$ kubectl run redis --image=redis -n finance
```
4. Which namespace has the `blue` pod in it?
```
$ kubectl get pods -A -o wide
```
