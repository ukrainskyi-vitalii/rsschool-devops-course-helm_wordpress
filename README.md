# DevOps Course. Terraform Infrastructure for AWS with GitHub Actions
# Task 5: Simple Application Deployment with Helm

This README file provides instructions for setting up and deploying a WordPress application using a custom Helm chart, which is configured to run on a Kubernetes cluster (K3s) deployed on AWS.

### 1. Deploy Required Infrastructure
The infrastructure, including VPC, subnets, security groups, and EC2 instance (k3s_master), is deployed using Terraform. K3S service and Helm is installed automatically during the k3s_master installation.

To deploy the infrastructure, run the following commands:

```bash
terraform init
terraform plan
terraform apply
```
This will provision the necessary resources on AWS.

### 2. # Install k3s
```bash 
curl -sfL https://get.k3s.io | sh -
```

### 3. Install Helm
```bash 
curl -L https://get.helm.sh/helm-v3.16.2-linux-amd64.tar.gz -o helm.tar.gz &&
tar -zxvf helm.tar.gz &&
sudo mv linux-amd64/helm /usr/local/bin/helm &&
rm -rf helm.tar.gz linux-amd64
```

### 4. Install kubectl
```bash 
curl -LO \"https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl\" &&
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl &&
rm kubectl
```

### 5. Clone and Deploy the Helm Chart
Pull the custom WordPress Helm chart repository:
```bash 
git clone https://github.com/your-github-repo/wordpress-chart.git
cd wordpress-chart
```

### 6. Customize Values for Deployment
Update values.yaml as needed, such as setting service.type to NodePort and specifying ports:

```bash 
service:
  type: NodePort
  nodePorts:
    http: 30080
    https: 30443
```

### 7. Deploy the Helm Chart
Deploy the chart to the Kubernetes cluster:
```bash 
helm install wordpress ./ -f values.yaml
```

### 8. Access the WordPress Application
The WordPress application should be accessible via the EC2 public IP address on the specified NodePort:
```bash
http://<EC2_Public_IP>:30080
```

### Alternative Way: Automated Deployment through CI/CD
For a fully automated deployment, we can use a CI/CD pipeline configured on GitHub Actions to manage infrastructure and application deployment.
