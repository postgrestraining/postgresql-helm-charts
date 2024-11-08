# PostgreSQL Replication on Minikube
This project sets up a PostgreSQL database cluster and a standalone PostgreSQL instance on a Minikube-based Kubernetes environment, including asynchronous replication from the cluster to the standalone instance.

## Requirements

** Minikube: A small Kubernetes cluster deployed on your local machine (2 CPUs, 2GB RAM, and 20GB disk).
** Helm: A package manager for Kubernetes, used to deploy and manage PostgreSQL deployments.
** Python: For running a data insertion script that populates PostgreSQL tables with 100,000 records using the Faker library.
