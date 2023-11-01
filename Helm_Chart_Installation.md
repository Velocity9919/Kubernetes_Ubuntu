-------------------------------------------------------- EKS-CLUSTER-CREATATION -----------------------------------------------------------

----------------------------------------- JAVA INSTALLATION -----------------------------------------
````
sudo apt-get update
sudo apt install openjdk-11-jre-headless
````
----------------------------------------- Install and Setup Jenkins -----------------------------------------
````
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

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
sudo vi /etc/sudoers
jenkins ALL=(ALL) NOPASSWD: ALL
````

----------------------------------------- MAVEN INSTALLATION -----------------------------------------
----------------------------------------- DOCKER INSTALLATION -----------------------------------------
````
sudo usermod -aG docker jenkins
sudo chmod 666 /var/run/docker.sock
````
-----------------------------------------
````
sudo su - jenkins
````
----------------------------------------- AWS CLI INSTALLATION -----------------------------------------
````
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
./aws/install -i /usr/local/aws-cli -b /usr/local/bin
aws --version
````

-----------------------------------------  Install eksctl on Ubuntu Linux -----------------------------------------
````
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
````

----------------------------------------- Installing or updating kubectl -----------------------------------------
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
----------------------------------------- HELM Installing -----------------------------------------
````
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
sudo chmod 700 get_helm.sh
sudo ./get_helm.sh
helm version --client
````
----------------------------------------- Helm chart creation -----------------------------------------
````
helm create mychart
````
----------------------------------------- AWS Configure -----------------------------------------
create IAM user with administration access
````
aws configure
````
AWS Access Key ID [None]: AKIATJQNTRM7NOULDJU6
AWS Secret Access Key [None]: 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
Default region name [None]: ap-south-1
Default output format [None]: json

----------------------------------------- Cluster creation -----------------------------------------
````
eksctl create cluster --name my-demo-eks --region ap-south-1 --nodegroup-name my-nodes --node-type t3.small --managed --nodes 2
````
````
kubectl get nodes
````
----------------------------------------- create Namespace -----------------------------------------
````
kubectl create ns helm-deployment
````
````
kubectl get ns
````
````
helm ls -n helm-deployment
````

````
eksctl get cluster --name my-demo-eks --region ap-south-1
````
````
aws eks update-kubeconfig --name my-demo-eks --region ap-south-1
````
Added new context arn:aws:eks:ap-south-1:226588068670:cluster/demo-eks to (/root/.kube/config)--> copy the path
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
kubectl get pods
````
````
kubectl get service
````
````
eksctl delete cluster --name my-demo-eks --region ap-south-1
````

````
eksctl create cluster --name naresh-test-cluster --version 1.27 --region ap-south-1 --nodegroup-name worker-nodes --node-type t2.micro --nodes 2
````
````
eksctl get cluster --name naresh-test-cluster --region ap-south-1
````
````
aws eks update-kubeconfig --name naresh-test-cluster --region ap-south-1
````
````
kubectl get nodes
````
````
kubectl get deployments
````
````
kubectl ls -n helm-deployment
````
````
kubectl get pods -n helm-deployment
````
````
kubectl get svc -n helm-deployment
````
````
kubectl describe pod first-mychart-6d44c4d969-b8n99 -n helm-deployment
````
````
kubectl get service 
````
````
eksctl delete cluster --name naresh-test-cluster --region ap-south-1
````
````
helm ls -n helm-deployment
````
````
kubectl get pods -n helm-deployment
````




kubectl get pods
kubectl top pods
kubectl node add --worker -p local-cluster

kubectl run nginx-pod --image=nginx



if status :
CrashLoopBackOff :
kubectl run spring-petclinic --image nareshbabu1991/spring-petclinic -- sleep infinity
kubectl get pods --watch

spring-petclinic

sh "helm upgrade first --install petclinic-chart --namespace helm-deployment --set image.tag=$BUILD_NUMBER"

helm upgrade first --install petclinic-chart/values-prod.yaml --namespace helm-deployment --set image.tag=$BUILD_NUMBER'
