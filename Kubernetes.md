## Kubernetes

Kubernetes is an opensource container orchestration system for automating application deployment, scaling and management.

Why?
* High availability or no downtime
* Scalability or high performance
* Disaster recovery(backup and restore)

## Architecture


<img width="1015" alt="Screen Shot 2021-01-03 at 1 33 14 PM" src="https://user-images.githubusercontent.com/60826485/103488901-84249300-4dde-11eb-8f88-fb52b0aea761.png">

## Installation

Hyperkit

* brew install hyperkit

MiniKube

* brew install minikube

minikube comes with kubectl

## Start virtual minikube cluster

* minikube start --vm-driver=hyperkit
* minikube status

To get status of nodes

* kubectl get nodes
* kubectl version 
* kubectl get services


## Create pods

create a pod

* kubectl create deployment nginx-depl --image=nginx
* kubectl get deployment
* kubectl get pod

Debugging pods
* kubectl describe pod podName
* kubectl logs podName
* kubectl exec -it podName -- bin/bash
* ls

Creating Configured Deployment

* kubectl apply -f fnginx-deployment.yaml

nginx-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80
```     

* kubectl apply -f nginx-deployment.yaml
~ created
* kubectl get deployment
* kubectl get pod
Update nginx-deployment.yaml 

* kubectl apply -f nginx-deployment.yaml
~ configured

## Example Mongo Application
### Create Deployment

mongo-deployment.yaml 
```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mongodb-deployment
      labels:
        app: mongodb
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mongodb
      template:
        metadata:
          labels:
            app: mongodb
        spec:
          containers:
            - name: mongodb
              image: mongo
              ports:
                - containerPort: 27017
              env:
                - name: MONGO_INITDB_ROOT_USERNAME
                  valueFrom:
                    secretKeyRef:
                      name: mongodb-secret
                      key: mongo-root-username
                - name: MONGO_INITDB_ROOT_PASSWORD
                  valueFrom: 
                    secretKeyRef:
                      name: mongodb-secret
                      key: mongo-root-password
```

### Create Secret
mongo-secret.yaml 
```
    apiVersion: v1
    kind: Secret
    metadata:
        name: mongodb-secret
    type: Opaque
    data:
        mongo-root-username: dXNlcm5hbWU=
        mongo-root-password: cGFzc3dvcmQ=
```
Encode:
>[Convert]::ToBase64String(echo -n 'username' | base64)

Decode:
>[System.Text.Encoding]::echo -n 'password' | base64
)

* kubectl apply -f mongo-secret.yaml  
* kubectl apply -f mongo-deployment.yaml  
* kubectl get pod --watch  
* kubectl describe pod podNAME

### Create Internal Service
Append to mongo-deployment.yaml:

    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: mongodb-service
    spec:
      selector:
        app: mongodb
      ports:
        - protocol: TCP
          port: 27017
          targetPort: 27017

* kubectl apply -f mongo-deployment.yaml  
* kubectl get service  
* kubectl descibe service serviceNAME  
* kubectl get pod -o wide

To see all components about mongodb
* kubectl get all | grep mongodb

### Create Mongo Express Deployment
Create mongo-express.yaml containing:

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mongo-express
      labels:
        app: mongo-express
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mongo-express
      template:
        metadata:
          labels:
            app: mongo-express
        spec:
          containers:
          - name: mongo-express
            image: mongo-express
            ports:
            - containerPort: 8081
            env:
            - name: ME_CONFIG_MONGODB_ADMINUSERNAME
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-username
            - name: ME_CONFIG_MONGODB_ADMINPASSWORD
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-password
            - name: ME_CONFIG_MONGODB_SERVER
              valueFrom:
                configMapKeyRef:
                  name: mongodb-configmap
                  key: database_url

### Create ConfigMap
Create mongo-configmap.yaml containing:

    apiVersion: v1
    kind: ConfigMap
    metadata:
        name: mongodb-configmap
    data:
        database_url: mongodb-service

* kubectl apply -f mongo-configmap.yaml  
* kubectl apply -f mongo-express.yaml

### Create External Service
Append to mongo-express.yaml:

    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: mongo-express-service
    spec:
      selector:
        app: mongo-express
      type: LoadBalancer
      ports:
        - protocol: TCP
          port: 8081
          targetPort: 8081
          nodePort: 30000

* kubectl apply -f mongo-express.yaml  
* kubectl get service  
* kubectl describe service mongodb-service  
* kubectl get pod -o wide  
* kubectl describe pod podID  
* kubectl logs podID  

Serve Mongo Express
* minikube service mongo-express-service
