------------------- EKS-CLUSTER-CREATATION ----------------------


------------------- Java Installation---------------------------

````
sudo hostnamectl set-hostname Jenkins
````
````
/bin/bash
````
````
sudo apt-get update -y
sudo apt-get install -y default-jdk
java --version
````

-------------------- Install and Setup Jenkins -------------------
````
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
````
````
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
````
````
sudo apt-get update
sudo apt-get install jenkins
````
````
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
````
````
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
````
````
sudo vi /etc/sudoers
````
````
jenkins ALL=(ALL) NOPASSWD: ALL
````

---------------------- Maven Installation -------------------------
````
cd /opt/
````
````
wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
````
````
tar -xvf apache-maven-3.9.4-bin.tar.gz
````
````
mv apache-maven-3.9.4 maven
````
````
rm apache-maven-3.9.4-bin.tar.gz
````
````
sudo vi /etc/profile.d/mavenenv.sh
````
````
export JAVA_HOME=/usr/lib/jvm/default-java

export M2_HOME=/opt/maven

export PATH=${M2_HOME}/bin:${PATH}
````
````
sudo chmod +x /etc/profile.d/mavenenv.sh
````
````
source /etc/profile.d/mavenenv.sh
````
````
mvn --version
````

---------------------- Docker Installation -------------------------
````
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
````
````
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
````
````
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
````
````
sudo apt-get update
````
````
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
````
````
service docker restart
````
````
sudo usermod -aG docker jenkins
sudo chmod 666 /var/run/docker.sock
````
````
sudo systemctl restart docker
````
````
sudo systemctl status docker
````


````
sudo su - jenkins
````

----------------- Install or update the AWS CLI --------------
````
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
./aws/install -i /usr/local/aws-cli -b /usr/local/bin
aws --version
````

------------------- Install eksctl on Ubuntu Linux ---------------
````
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
````

---------------- Installing or updating kubectl -------------------

````
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl.sha256
sha256sum -c kubectl.sha256
openssl sha1 -sha256 kubectl
chmod +x ./kubectl
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
kubectl version --short --client
````
------------------------ AWS Configure --------------------------

create IAM user with administration access
````
aws configure
````
AWS Access Key ID [None]: AKIATJQNTRM7NOULDJU6
AWS Secret Access Key [None]: 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
Default region name [None]: ap-south-1
Default output format [None]: json

------------------------------ Cluster creation ----------------------- 

````
eksctl create cluster --name naresh-test-cluster --region ap-south-1 --nodegroup-name my-nodes --node-type t3.small --managed --nodes 2
````
````
eksctl get cluster --name naresh-test-cluster --region ap-south-1
````
````
aws eks update-kubeconfig --name naresh-test-cluster --region ap-south-1
````

Added new context arn:aws:eks:ap-south-1:226588068670:cluster/demo-eks to (/root/.kube/config)--> copy the path
(crete credencials with this config file)
````
cat /root/.kube/config 
````
````
kubectl get nodes
````
````
kubectl get deployments
````
````
kubectl get service
````
````
eksctl delete cluster --name naresh-test-cluster --region ap-south-1
````
