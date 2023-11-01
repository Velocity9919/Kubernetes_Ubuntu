
--------------------------------- Installing or updating kubectl ---------------------------------------
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
-------------------------------- Install using the apt repository --------------------------------------
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
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
````

````
sudo vi /etc/sudoers
````
````
minikube ALL=(ALL) NOPASSWD: ALL
````
````
sudo usermod -aG docker minikube
sudo chmod 666 /var/run/docker.sock
````
------------------------------------------------------------------------------------------------------------
````
sudo su - minikube
````
--------------------------------------------- install minikube ---------------------------------------------
````
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
````
````
minikube version
````
````
minikube start --nodes 2 -p local-cluster --driver=docker
````
````
minikube status -p local-cluster
````
````
minikube add node --worker -p local-cluster (it will add worker node for the cluster)
````
````
minikube delete node (node name) -p local-cluster (it will delete the worker node for the cluster)
````
````
minikube dashboard --url -p local-cluster  (it will give the url of minikube to access on the brouser)
````
