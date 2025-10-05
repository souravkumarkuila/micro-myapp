🚀 Kubernetes Deployment with ArgoCD

This repository contains the Kubernetes manifests and configuration files used to deploy applications using ArgoCD — a declarative GitOps continuous delivery tool for Kubernetes.

🧠 Overview

ArgoCD is a GitOps tool that continuously monitors your Git repository and automatically syncs application state in Kubernetes clusters based on the declared configuration.

In this setup:

We create a dedicated namespace for ArgoCD.

Install ArgoCD using official manifests.

Expose the ArgoCD UI.

Assign necessary permissions for ArgoCD to manage cluster resources.

Deploy our Kubernetes project via ArgoCD.

🧩 Prerequisites

Before you begin, make sure you have:

✅ A running Kubernetes cluster

✅ kubectl CLI configured and connected to your cluster

✅ Internet access to pull manifests from GitHub

✅ Admin privileges on the cluster

⚙️ Setup Instructions
1️⃣ Create ArgoCD Namespace
kubectl create namespace souravkkargocd

2️⃣ Install ArgoCD in the Namespace
kubectl apply -n souravkkargocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


This command installs all ArgoCD components (server, controller, repo-server, etc.) in the souravkkargocd namespace.

3️⃣ Expose ArgoCD UI (Port Forward)

To access the ArgoCD web interface locally:

kubectl port-forward svc/argocd-server -n souravkkargocd 7000:443


Now open your browser and navigate to:
👉 https://localhost:7000

4️⃣ Get Initial Admin Password

ArgoCD sets up a default admin password as the name of the argocd-initial-admin-secret secret.

Run the following command to retrieve it:

argocd admin initial-password -n souravkkargocd


Copy the password and log in to the UI:

Username: admin

Password: (output of the above command)

5️⃣ Grant Cluster Admin Access to ArgoCD

ArgoCD needs cluster-wide permissions to manage deployments across namespaces.

Run:

kubectl create clusterrolebinding argocd-admin --clusterrole=cluster-admin --serviceaccount=souravkkargocd:argocd-application-controller

🚀 Deploying Your Application via ArgoCD

Go to the ArgoCD UI → New App

Enter the following details:

Application Name: your-app-name

Project: default

Repository URL: https://github.com/<your-username>/<your-repo>.git

Path: path to your Kubernetes manifests (e.g., k8s/ or manifests/)

Cluster URL: https://kubernetes.default.svc

Namespace: namespace where you want to deploy

Click Create → then Sync

ArgoCD will now automatically deploy and manage your application.
