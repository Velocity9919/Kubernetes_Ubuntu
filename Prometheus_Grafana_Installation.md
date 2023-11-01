---------------------------------------------- How to setup Prometheus, Grafana dashboard for Kubernetes monitoring on AWS --------------------------------------------
---------------------------------------------- AWS Cli installation ----------------------------------------------
````
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
./aws/install -i /usr/local/aws-cli -b /usr/local/bin
aws --version
````
----------------------------------------------  Install eksctl on Ubuntu Linux ----------------------------------------------
````
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
````

---------------------------------------------- AWS Configure ----------------------------------------------
create IAM user with administration access
````
aws configure
````
AWS Access Key ID [None]: AKIATJQNTRM7NOULDJU6
AWS Secret Access Key [None]: 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
Default region name [None]: ap-south-1
Default output format [None]: json


---------------------------------------------- Installing or updating kubectl ----------------------------------------------
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

---------------------------------------------- HELM Installing ----------------------------------------------
````
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
sudo chmod 700 get_helm.sh
sudo ./get_helm.sh
helm version --client
````

---------------------------------------------- Create Kubernetes cluster in AWS using eksctl ----------------------------------------------

````
eksctl create cluster --name test-cluster-1 --version 1.27 --region ap-south-1 --nodegroup-name worker-nodes --node-type t2.large --nodes 2 --nodes-min 2 --nodes-max 3
````
````
kubectl get nodes
````
````
eksctl get cluster --name test-cluster-1 --region ap-south-1
````
````
aws eks update-kubeconfig --name test-cluster-1 --region ap-south-1
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
kubectl get service
````
---------------------------------------------- Install Kubernetes Metrics Server ----------------------------------------------
````
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
````
````
kubectl get deployment metrics-server -n kube-system
````

---------------------------------------------- Install Prometheus ----------------------------------------------
````
helm repo add stable https://charts.helm.sh/stable
````
````
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
````
````
kubectl create namespace prometheus
````
````
helm install prometheus prometheus-community/prometheus \
    --namespace prometheus \
    --set alertmanager.persistentVolume.storageClass="gp2" \
    --set server.persistentVolume.storageClass="gp2"
````
````
kubectl get all -n prometheus
````

View the Prometheus dashboard by forwarding the deployment ports
kubectl port-forward deployment/prometheus-server 9090:9090 -n prometheus


---------------------------------------------- Install grafana ----------------------------------------------

````
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
````
````
vi  prometheus-datasource.yaml
````
````
datasources:
  datasources.yaml:
    apiVersion: 1
    datasources:
    - name: Prometheus
      type: prometheus
      url: http://prometheus-server.prometheus.svc.cluster.local
      access: proxy
      isDefault: true
````
````
kubectl create namespace grafana
````
````
helm install grafana grafana/grafana \
    --namespace grafana \
    --set persistence.storageClassName="gp2" \
    --set persistence.enabled=true \
    --set adminPassword='EKS!sAWSome' \
    --values prometheus-datasource.yaml \
    --set service.type=LoadBalancer
````
````
kubectl get all -n grafana
````
````
kubectl get service -n grafana 
````
Copy the Public AWS IP address and open it in the browser -----------

---------------------------------------------- Import Grafana dashboard from Grafana Labs ----------------------------------------------
https://grafana.com/grafana/dashboards/ ---> to dashboard

I am going to import a dashboard 6417

after import
Now you can see your Grafana dashboard displaying all the Kubernetes metrics.

Deploy a spring boot microservice and monitor it on Grafana
````
kubectl apply -f k8s-spring-boot-deployment.yml 
````
````
kubectl get deployment
````
````
eksctl delete cluster --name test-cluster-1 --region ap-south-1
````

