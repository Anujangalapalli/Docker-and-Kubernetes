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






