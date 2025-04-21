## Kubernetes Cluster Setup Guide

### Steps

-   [1. SSH into the Node (Instance)](#1-ssh-into-the-node-instance)
-   [2. Configure Infrastructure](#2-configure-infrastructure)
    -   [Disable Swap](#disable-swap)
    -   [Set Host Names of Nodes](#set-host-names-of-nodes)
    -   [Assign Hostnames](#assign-hostnames)
-   [3. Initialize Kubernetes Cluster](#3-initialize-kubernetes-cluster)
-   [4. Verify Kubelet Service](#4-verify-kubelet-service)
-   [5. Configure kubectl Access for Admin](#5-configure-kubectl-access-for-admin)
-   [6. Basic kubectl Commands](#6-basic-kubectl-commands)
-   [7. Install Pod Network Plugin (Weave Net)](#7-install-pod-network-plugin-weave-net)
-   [8. Join Worker Nodes](#8-join-worker-nodes)
-   [9. Test Deployment](#9-test-deployment)

### 1. SSH into the Node (Instance)

We first connect to each node (server instance) so we can configure them directly.

**On any node:**

```bash
ssh user@<node-ip>
```

### 2. Configure Infrastructure

**Disable Swap**
Kubernetes requires swap to be disabled for performance and scheduling consistency.

**Run on All Nodes:**

```bash
sudo swapoff -a
```

**Set Host Names of Nodes**
We set hostnames and update `/etc/hosts` so nodes can communicate using names instead of IPs, making the setup easier to manage.

**Run on All Nodes:**

```bash
sudo vim /etc/hosts
```

Add:

```bash
<node-ip> <node-name>
```

Example:

```bash
192.168.1.10 master
192.168.1.11 worker1
192.168.1.12 worker2
```

**Assign Hostnames**
Assign a unique hostname to each server so Kubernetes can identify them clearly.
**On Master:**

```bash
sudo hostnamectl set-hostname master
```

**On Worker1:**

```bash
sudo hostnamectl set-hostname worker1
```

**On Worker2:**

```bash
sudo hostnamectl set-hostname worker2
```

### 3. Initialize Kubernetes Cluster

This sets up the control plane components (API server, scheduler, etc.) on the master node.

**Run on Master:**

```bash
sudo kubeadm init
```

### 4. Verify Kubelet Service

`kubelet` is the main Kubernetes node agent; it must be running on all nodes.
**Run on All Nodes:**

```bash
service kubelet status
systemctl status kubelet
```

**View logs for troubleshooting:**

```bash
journalctl -u kubelet
```

### 5. Configure kubectl Access for Admin

We copy the Kubernetes admin config so we can run kubectl commands as the current user.

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 6. Basic kubectl Commands

These commands help verify that the cluster is up and pods are running.
**Get node information:**

```bash
kubectl get nodes
```

**Get pods in kube-system namespace:**

```bash
kubectl get pods -n kube-system
```

**Get pods from all namespaces:**

```bash
kubectl get pod -A
```

**Get detailed output:**

```bash
kubectl get pod -n kube-system -o wide
#or
kubectl get pod -A -o wide
```

### Install Pod Network Plugin (Weave Net)

Kubernetes nodes need a network plugin so pods can communicate across nodes. Here, we install Weave Net.
**Install plugin:**
**Run on Master:**

```bash
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

**Check network status:**

```bash
kubectl exec -n kube-system weave-net-1jkl6 -c weave -- /home/weave/weave --local status
```

### 8. Join Worker Nodes

Workers need container runtime and a join command to become part of the cluster.

**Install containerd (Run on All Workers):**

```bash
vim install-containerd.sh
chmod u+x install-containerd.sh
./install-containerd.sh
```

**On Master — Generate join command:**

```bash
kubeadm token create --help
kubeadm token create --print-join-command
```

**On Workers — Run the join command as ROOT:**

```bash
sudo kubeadm join 172.31.43.99:6443 --token 9bds1l.3g9ypte9gf69b5ft --discovery-token-ca-cert-hash sha256:xxxx
```

### 9. Test Deployment

We deploy an Nginx pod to confirm the cluster is working properly.

```bash
kubectl run test --image=nginx
```
