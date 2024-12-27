# Minikube-Cluster-Setup
This guide explains how to set up Minikube (a tool for running Kubernetes clusters locally) on an Ubuntu AWS EC2 instance. It includes steps for installing Minikube, Docker, kubectl, and configuring the Minikube Cluster.
## Prerequisites
### Ubuntu EC2 Instance:

  - Instance type: t2.medium or larger (for multi-node clusters, use t2.large or above).
  - Docker and Kind require at least 4 GB RAM.
  - Ubuntu version: 20.04 or later.
### Access to the Instance:
  - SSH access to the AWS EC2 instance.

### Step 1: Update and Install Dependencies
  1) Update the system:

```bash
sudo apt update && sudo apt upgrade -y
```
  2) Install dependencies:

```bash

sudo apt install -y curl apt-transport-https ca-certificates software-properties-common
```

### Step 2: Install Required Tools
#### 2.1 Install Docker
1) Install Docker:

```bash

sudo apt update
sudo apt install docker.io -y
```
2) Start and enable Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```
3) Add your user to the Docker group:

```bash

sudo usermod -aG docker $USER
```
4) Verify Docker installation:

```bash
docker --version
```

#### 2.2 Install kubectl
1) Download kubectl:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
2) Make it executable and move it to your PATH:

```bash

chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
3) Verify kubectl installation:

```bash

kubectl version --client
```

#### 2.3 Install Minikube
1) Install dependencies for Minikube:

```bash
sudo apt install -y apt-transport-https curl
```
2) Download Minikube:

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.28.0/minikube-linux-amd64
```
3) Make Minikube executable and move it to your PATH:

```bash
chmod +x minikube
sudo mv minikube /usr/local/bin/
```
4) Verify Minikube installation:

```bash

minikube version
```

### Step 3: Set Up Minikube Cluster
#### 3.1 Start Minikube
1) Start Minikube:

```bash
minikube start --driver=docker
```
2) Verify Minikube status:

```bash
minikube status
```
3) Check the Kubernetes cluster:

```bash
kubectl cluster-info
```
4) Verify the nodes:

```bash
kubectl get nodes
```
#### 3.2 Access the Minikube Dashboard
1) Launch the Minikube dashboard:

```bash
minikube dashboard
```
This will open the Kubernetes dashboard in your web browser.

### Step 4: Interact with the Minikube Cluster
#### 4.1 Deploy a Sample Application
1) Create a simple Nginx Deployment:

```bash
kubectl create deployment nginx --image=nginx
```
2) Expose the deployment as a service:

```bash
kubectl expose deployment nginx --type=NodePort --port=80
```
3) Check the services:

```bash
kubectl get svc
```
4) Access the application: Get the NodePort assigned to the Nginx service:

```bash
kubectl get svc nginx
```
Access the application using the following URL:

```php
http://<Minikube-IP>:<NodePort>
```
Replace <Minikube-IP> with the output of:

```bash

minikube ip
```


### Step 5: Stop and Delete the Minikube Cluster
**Stop the Minikube cluster:**

```bash
minikube stop
```
**Delete the Minikube cluster:**

```bash
minikube delete
```
## Notes
  - Minikube Cluster in AWS: Minikube is typically used for local clusters. If you plan to use it in the cloud, consider setting up a multi-node Kubernetes cluster with tools like Kops or EKS.
  - Access Minikube Dashboard: The Minikube dashboard is accessible only within the machine running Minikube. If you want remote access, consider using port forwarding.

## Troubleshooting
  - Minikube issues:

    Restart Minikube if the cluster fails to start:
    ```bash

    minikube start --driver=docker
    ```
  - Docker issues:

    Restart Docker:
    ```bash

    sudo systemctl restart docker
    ```
## References
- [Minikube Official Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes Official Documentation](https://kubernetes.io/docs/)

