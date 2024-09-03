# Netflix Application Deployment.  (DevSecOps-Project)

<div align="center">
  <img src="./public/assets/net-1.png" alt="Logo" width="100%" height="100%">
</div>
<br>

<div align="center">
  <img src="./public/assets/net-2.png" alt="Logo" width="100%" height="100%">
</div>
<br>

<div align="center">
  <img src="./public/assets/net-4.png" alt="Logo" width="100%" height="100%">
</div>

### Create the Ec2 Instance As shown in below.

<div align="center">
  <img src="./public/assets/Ec2Instance.png" alt="Logo" width="100%" height="100%">
</div>
<br>

### Open the below mentioned port number open as shown in the image.
<div align="center">
  <img src="./public/assets/PortNumberUsed.png" alt="Logo" width="100%" height="100%">
</div>
<br>

## Installing the Jenkins. (I am here following master nodeagent config.)

1. Create the 2 EC2 instance  ubuntu machine with 15GB memory storage and run the below command on both machine.

```
sudo apt update
sudo apt upgrade -y
```

```
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment (build 17.0.8+7-Debian-1deb12u1)
OpenJDK 64-Bit Server VM (build 17.0.8+7-Debian-1deb12u1, mixed mode, sharing)

#jenkins
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

```

### Install docker on both the machine 
```
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER  # Replace with your system's username, e.g., 'ubuntu'
newgrp docker
sudo chmod 777 /var/run/docker.sock
```

### Install the trivy on NodeAgent jenkins Machine 
```
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

### To resolve the docker login issue on jenkins pipeline machine, Run the below coomand on the both jenkins machine.
```
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

# To access the masterJenkins Machine 'publicIP:8080'
###### We are doing all operation in master node & all operation run in jenkinsNode.

After this install required plugins for this project.
1. Eclipse
2. docker
3. sonarqube scanner
4. quality gate
5. dependency check (OWASP)
6. prometheus
7. nodejs

----> Restart the jenkins machine after this if required.

#### Add the below mentioned credential in the jenkins machine
<div align="center">
  <img src="./public/assets/Credential.png" alt="Logo" width="100%" height="100%">
</div>
<br>

### Install the SonarQube machine .
###### Create ubuntu EC2 machine and install the docker on that machine . And Execute the below commnad.
```
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

###### Access this --> PublicIP:9090 and you will get like this. And use 'admin' as a username and password for 1st. And Change to new one.

<div align="center">
  <img src="./public/assets/SonarQube.png" alt="Logo" width="100%" height="100%">
</div>
<br>

---> Here Create a new project named as 'Netflix' and setup it Manual. \
---> And Also create a webhook , 'publicIPJenkins:8080/sonarqube-webhook/' 

### ------------------ Setup Bootstrap Server for eksctl and Setup Kubernetes using eksct----------------------


###### Create a EC2 instance with t2.large machine with 20GB space storage. (ubuntu)

## Install AWS Cli on the above EC2
```
sudo su
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
apt install unzip,   $ unzip awscliv2.zip
sudo ./aws/install
```

or

```
sudo yum remove -y aws-cli
pip3 install --user awscli
sudo ln -s $HOME/.local/bin/aws /usr/bin/aws
aws --versio
```
##### After this configure the awscli
```
aws configure
```
Give here ,
----> AWS Access_Key : ********************
----> AWS Secret_Key : ********************

## Installing kubectl (Install below CMD 1 By 1)

```
sudo su
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
ll
chmod +x ./kubectl  //Gave executable permisions
mv kubectl /bin   //Because all our executable files are in /bin
 kubectl version --output=yaml
```

## Installing  eksctl (Install below CMD 1 By 1)
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
cd /tmp
ll
sudo mv /tmp/eksctl /bin
eksctl version
```
## Setup Kubernetes using eksctl
```
 eksctl create cluster --name virtualtechbox-cluster \
--region ap-south-1 \
--node-type t2.large \
--nodes 1 \
```

### Varify !!
```
kubectl get nodes
```

# --------------- Install helm on this machine ----------------------------------

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Install Node Exporter using Helm.
1. Install Node Exporter using Helm
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

2. Create a Kubernetes namespace for the Node Exporter:
```
kubectl create namespace prometheus-node-exporter
```

3.kubectl create namespace prometheus-node-exporter
```
helm install prometheus-node-exporter prometheus-community/prometheus-node-exporter --namespace prometheus-node-exporter
```

#### And check
```
kubectl get svc
```
You will find ns with prometheus.


<div align="center">
  <img src="./public/assets/kubectlgetsvc-n-prometheus.png" alt="Logo" width="100%" height="100%">
</div>
<br>


or

#### Links to download Prometheus, Node_Exporter & black Box exporter . https://prometheus.io/download/   (Follow this, I am followed this.)



## ========= ArgoCD Installation on EKS Cluster and Add EKS Cluster to ArgoCD ==========

#### Follow this link --> https://archive.eksworkshop.com/intermediate/290_argocd/install/
or 
#### Execute the below command
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml
```

```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

Wait about 2 minutes for the LoadBalancer creation



```
export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`
```

###### You will get argocd URL , copy and paste in chrome 'admin' default username.
```
echo $ARGOCD_SERVER
```

```
export ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
```

###### password for login
```
echo $ARGO_PWD
```

# After Login 

###### Use the default cluster
<div align="center">
  <img src="./public/assets/argocdDefaultCluster.png" alt="Logo" width="100%" height="100%">
</div>
<br>

###### Use the git repository of your project

<div align="center">
  <img src="./public/assets/argocdgithub.png" alt="Logo" width="100%" height="100%">
</div>
<br>

###### Create a app with default cluster.

<div align="center">
  <img src="./public/assets/argocdCluster.png" alt="Logo" width="100%" height="100%">
</div>
<br>




