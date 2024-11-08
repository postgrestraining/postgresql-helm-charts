# PostgreSQL Set-up on Minikube
This project sets up a PostgreSQL database cluster and a standalone PostgreSQL instance on a Minikube-based Kubernetes environment, including asynchronous replication from the cluster to the standalone instance.

### Requirement

1. Using Minikube, deploy a small Kubernetes cluster in your local environment. This won’t require a lot of resources, 2 CPU 2 GB RAM and 20GB disk space will do. 
2. Using Helm Chart, deploy a PostgreSQL database cluster with a load balancer. 
3. You are not limited to the number of nodes in the cluster, decide on the number based on your machine’s resource availability. 
4. You are not limited to the specs for the nodes e.g mem, storage, vCPU. Decide on the number based on your machine’s resource availability. 
5. Develop a script/code to create a simple database with two related tables using a foreign key. 
6. Using the Faker library, or any other random data generation library you are familiar with, insert 100,000 records into the two tables. 
7. Using Helm Chart still, deploy a standalone PostgreSQL database, then setup an async replication from your cluster to this standalone database. 
8. Push your codes to Github.

### Expected deliverables

1. An architectural diagram showing your solution architecture, together with documentation. 
2. A Git repo containing Helm Charts and codes/scripts to deploy a PostgreSQL database cluster with a load balancer, and schema/tables/records. Senior Database Administrator Assignment 1 
3. PostgreSQL database cluster with a load balancer, and schema/tables/records. 
4. The standalone replica database needs to be in sync with the main cluster showing the replicated records. 

## Setup requirements

-  CentOS 9: Virtual Machine for Minikube setup.
-  Minikube: A small Kubernetes cluster deployed on your local machine (2 CPUs, 2GB RAM, and 20GB disk). -
-  Helm: A package manager for Kubernetes, used to deploy and manage PostgreSQL deployments.
-  ython: For running a data insertion script that populates PostgreSQL tables with 100,000 records using the Faker library.

## Setup instructions and sample outputs

The following set up is ran on Cent OS 9 Virtual machine.

## Pre-requisites

### 1. Install PostgreSQL Client on VM

```
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
dnf -qy module disable postgresql
dnf install -y postgresql17-server
```
### 2. Install and docker

```
dnf install -y dnf-plugins-core
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
dnf install -y docker-ce docker-ce-cli containerd.io
systemctl enable --now docker
systemctl start docker
```

After successful installation, docker status should look like below

```
[root@lab01 ~]# systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
     Active: active (running) since Sat 2024-11-09 01:05:43 IST; 6s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 37006 (dockerd)
      Tasks: 9
     Memory: 23.6M
        CPU: 156ms
     CGroup: /system.slice/docker.service
             └─37006 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Nov 09 01:05:41 lab01 dockerd[37006]: time="2024-11-09T01:05:41.566266827+05:30" level=info msg="Loading containers: start."
Nov 09 01:05:41 lab01 dockerd[37006]: time="2024-11-09T01:05:41.595896119+05:30" level=info msg="Firewalld: created docker-forwarding >
Nov 09 01:05:42 lab01 dockerd[37006]: time="2024-11-09T01:05:42.916140107+05:30" level=info msg="Firewalld: interface docker0 already >
Nov 09 01:05:43 lab01 dockerd[37006]: time="2024-11-09T01:05:43.107691425+05:30" level=info msg="Loading containers: done."
```
### 3. Install Python to load the data

```
sudo dnf install -y python3
dnf install python3-pip -y
pip3 install faker psycopg2-binary
```

### 4. Install and Start Minikube

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=docker --cpus=2 --memory=2048mb --disk-size=20g --force
```
To check if it has successfully configured run `minikube status` and you get something like below

```
[root@lab01 ~]# minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### 5. Install kubectl

```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Verify the installation

```
[root@lab01 ~]# kubectl version --client
Client Version: v1.31.0
Kustomize Version: v5.4.2
[root@lab01 ~]#
```

### 6. Install and Configure Helm

```
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

Output for the last command should be something like

```
[root@lab01 ~]# helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈
[root@lab01 ~]#
```

### 7. Download the helm charts to deploy PostgreSQL

```
sudo dnf install -y git
git clone https://github.com/postgrestraining/postgresql-helm-charts.git
cd postgresql-helm-charts
```

## PostgreSQL Deployment

### 1. Deploy PostgreSQL Database Cluster with Helm

```
cd /root/postgresql-helm-charts
helm install primary-postgresql ./chart-primary
```

Check if the deployment is successful, the output should look like this 

```
[root@lab01 postgresql-helm-charts]# helm install primary-postgresql ./chart-primary
NAME: primary-postgresql
LAST DEPLOYED: Sat Nov  9 01:23:46 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

[root@lab01 postgresql-helm-charts]# kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
primary-postgresql-66bd47f89-jnpnj   1/1     Running   0          42s

[root@lab01 postgresql-helm-charts]# kubectl get svc
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes           ClusterIP      10.96.0.1       <none>        443/TCP          13m
primary-postgresql   LoadBalancer   10.105.10.137   <pending>     5432:30467/TCP   45s
[root@lab01 postgresql-helm-charts]#
```



