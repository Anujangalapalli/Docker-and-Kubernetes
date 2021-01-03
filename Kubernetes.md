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
