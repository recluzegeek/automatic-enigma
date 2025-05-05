# Kubernetes

Pods are the smallest unit in the k8s world, as container in the docker world. Nodes are physical or virtual machines on which containers would run

## Kubernetes Services

Each node has kublet-proxy, container-runtime, and kubelet.

worker nodes communicates with api service on master nodes via kublet service
kube-proxy service present on each node is responsible for network communication bw worker and master nodes, and between worker nodes as well

API server service => is the main point of communication bw nodes in k8s world. worker nodes communicate with master node
scheduler service => planing and distribution of loads of different nodes in cluster
kube controller manager => is the single point which controls everything in the k8s cluster, and it also handles what actually happens on each of node of the cluster
cloud controller manager => interacts with cloud service providers where the cluster is residing, assist in the creation and management of
etcd => stores log related to nodes as key value pairs

## How Kubernetes Works

Here’s a simple flow of how Kubernetes handles things:

1. You create a Pod: You define what containers you want and how you want them to run (like specifying resources or settings).

1. Kubernetes schedules the Pod: The Scheduler decides which worker node should run the pod based on available resources.

1. Kubernetes manages the Pod: The Kubelet on the worker node makes sure the pod is running, and the Kube-proxy makes sure that networking works between pods and external traffic.

1. Scaling: If you want to run more copies of a pod (say you want 5 copies of a web service for more traffic), Kubernetes can automatically make more copies for you. This is called scaling.

1. Self-Healing: If a pod fails or crashes, Kubernetes automatically detects it and restarts or replaces it. This ensures that your application is always running, even if something goes wrong.

## Persistent Storage

By default, containers (and pods) are ephemeral (temporary). If a pod goes down, any data inside it is lost. Kubernetes solves this problem with Persistent Volumes. These are storage resources (like disks) that you can attach to your pods, ensuring data is saved even if the pod crashes.

## Cluster Management

done via `kubectl`

## Minikube

Minikube is a single node kubernetes cluster designed for learning purposes, where it acts as the master and worker nodes at the same time. It support various runtimes such as docker, podman, virtual box, vmware and others as well.

```bash
minikube version

minikube start --driver=virtualbox
minikube status

kubectl cluster-info
```
