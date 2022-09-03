### Flow :

### Git --> Git-Hub --> Maven build --> copy Jar to S3 bucket --> Build Docker Image --> Push that image to AWS-ECR --> Pull that Image from ECR using Ansible Playbook and deploy to Servers--> Send Slack Notification about Build-Status

### Tools Used : Git,Git-Hub,Maven ,Docker,Ansible, Slack . AWS Servives - EC2, IAM Roles, AWS ECR , AWS S3

1) Install Java, unzip , maven , docker, ansible, aws-cli, jenkins nad setup all tools
#!/bin/bash
sudo apt update
sudo apt install -y unzip openjdk-11-jdk
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
ansible-config init --disabled -t all > /etc/ansible/ansible.cfg
sed -i 's/;host_key_checking=True/host_key_checking=False/g' /etc/ansible/ansible.cfg
curl https://get.docker.com | bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz
tar xzvf apache-maven-3.8.6-bin.tar.gz
usermod -aG docker jenkins
sudo systemctl daemon-reload
sudo systemctl restart docker
service jenkins restart

java --version && ansible --version && docker version && aws --version && maven --version

# Plugins to Install
---> Global tool Configuration--> Maven --> /opt/apache-maven-3.8.6
1) Ansible
2) Docker Pipeline
3) AWS Steps
4) Slack Notifications
Configure System --> Slack--> Workspace-->Add Creds as secret text(token)-->Channel --> Enable Custom slack app bot user


2) Take Java code and try to build with maven code . It will build jar file
3) Copy Jar file to S3 bucket
4) Write Docker file to start a container and run jar file
4) push docker image to AWS ECR
5) Create another server (with docker and aws-cli installed on it and role assigned) and establish connection between two servers with ansible
#!/bin/bash
sudo useradd -m ansible --shell /bin/bash
sudo mkdir -p /home/ansible/.ssh
sudo chown -R ansible /home/ansible/
sudo touch /home/ansible/.ssh/authorized_keys
sudo usermod -aG root ansible
echo 'ansible ALL=(ALL) NOPASSWD: ALL' | sudo tee -a /etc/sudoers
echo '<pub-key>' | sudo tee /home/ansible/.ssh/authorized_keys
sudo apt update
sudo apt install -y unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
curl https://get.docker.com | bash

6) pull docker image from ECR and deploy on new server with help of ansible

Note : Refer-Jenkins file and Playbook for reference

access app with below URL :

http://34.237.223.249:8080/course-svc/getAllDevopsTools
