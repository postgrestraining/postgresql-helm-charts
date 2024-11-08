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

-  Minikube: A small Kubernetes cluster deployed on your local machine (2 CPUs, 2GB RAM, and 20GB disk). -
-  Helm: A package manager for Kubernetes, used to deploy and manage PostgreSQL deployments.
-  ython: For running a data insertion script that populates PostgreSQL tables with 100,000 records using the Faker library.
