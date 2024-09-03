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

