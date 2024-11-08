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

After successful installation, docker status should show as below

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
