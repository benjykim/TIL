# KodeKloud-PracticeTest-Services

1. How many Services exist on the system? in the current(default) namespace
```
$ kubectl get svc -n default
```
2. That(result of 1st command) is a default service created by Kubernetes at launch.
```
NAME      TYPE       CLUSTER-IP EXTERER-IP PORT(S) AGE
kubernetes ClusterIP 10.96.0.1  <none>     443/TCP 7m12s
```
3. What is the type of the default `kubernetes` service?
```
ClusterIP
```
4. What is the `targetPort` configured on the `kubernetes` service?
```
$ kubectl describe svc kubernetes
```
5. How many labels are configured on the `kubernetes` service?
```
$ kubectl describe svc kubernetes
```
6. How many Endpoints are attached on the `kubernetes` service?
```
$ kubectl describe svc kubernetes
``` 
7. How many Deployments exist on the system now? in the current(default) namespace
```
$ kubectl get deploy -n default
```
8. What is the image used to create the pods in the deployment?
```
$ kubectl describe deploy simple-webapp-deployment 
```
9. Are you able to accesss the Web App UI? Try to access the Web Application UI using the tab simple-webapp-ui above the terminal.
```
No
``` 
10. Create a new service to access the web application using the service-definition-1.yaml file
`Name:` webapp-service  
`Type:` NodePort  
`targetPort:` 8080  
`port:` 8080  
`nodePort:` 30080  
`selector:` simple-webapp
```
apiVersion: v1
kind: Service
metadata: 
  name: webapp-service
spec: 
  type: NodePort
  ports: 
    - targetPort: 8080
      port: 8080
      nodePort: 30080
  selector: 
    name: simple-webapp
```
