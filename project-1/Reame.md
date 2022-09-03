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

java --version && ansible --version && docker version && aws --version && maven --version

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
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDh/9PjDLwX6AzRKmrG01iBCJ3ffEAe68aYDB1sBStGT9SsXvZuJkq1iHRl0R3sQvkpKDjc3Gt4gc25/fUN1/qIOA6k9a1hboySzoeY3SfL9UR8T4Fii1/zBtbMZ8o8gyJUXhLex3ETCx/cuVdrJJUGsRKV14stiWWAJHzJlueVtOAzx9WuYZEv1JmrTcgkF+WYoxiklR4gJqXq5Eu2J1hWfxqXsBdcq9mbI+gkBKxemCSefw7eqlc9zB2Qqdv+8mwJN2SxE36P8bDRge/2Fsu4RyyIOzKK70c3JqAzNSPKtJiklCX3E6VXffn7zYw0NYyj7mkQN1WmXBqnCKvQHMgNRxkN9pamrCUlWUZG1f4vAhhGU+Xybu2A5RHs8abxCM5/MMGtQe8r6sALN76rlZADSJ3Z9x9rfIs5Y14qUu1R9kBYXHN4UAWW2Eozk9XPKdrVwRalqI3HkSWgVVDaAY+WUVeXExrLCeqQxLF0OaGpr+dN90LTdpDhd6tDEDZuvkU=' | sudo tee /home/ansible/.ssh/authorized_keys
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