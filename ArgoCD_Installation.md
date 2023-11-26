## ARGOCD INSTALLATION

1 ) First, create a namespace
````  
kubectl create namespace argocd
````
2 ) Next, let's apply the yaml configuration files for ArgoCd
````    
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
````
3 ) Now we can view the pods created in the ArgoCD namespace.
````    
kubectl get pods -n argocd
````
4 ) To interact with the API Server we need to deploy the CLI:
````
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
````
5 ) Expose argocd-server
````  
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
````
6 ) Wait about 2 minutes for the LoadBalancer creation
````
kubectl get svc -n argocd
````
7 ) Get pasword and decode it.
````    
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
````
````
echo QjZJYmZBbXpVbnRoY2ZiZA== | base64 --decode
````
8 ) Login in to the ArgoCd Dashboard using LoadBalancer
````    
a01c5477ca74643b09d78d18840a99b1-800807607.ap-south-1.elb.amazonaws.com
````
````
user name : admin
passwd : B6IbfAmzUnthcfbd (witch is the decoded passwd )
````	
go to the argocd --> settings --> update passwd --> admin@123

## Add EKS Cluster to ArgoCD

9 ) login to ArgoCD from CLI using cluster LoadBalancer url 
````
argocd login a01c5477ca74643b09d78d18840a99b1-800807607.ap-south-1.elb.amazonaws.com --username admin
````
````	
passwd : admin@123
````	
10) To Check clusters list
````
argocd cluster list
````	
	it will show default argocd cluster & also you can check cluster list on argocd --> settings --> clusters

11) Below command will show the EKS cluster
````
kubectl config get-contexts
````
13) Add above EKS cluster to ArgoCD with below command (NAMESPACE + CLUSTER NAME)
````
argocd cluster add ramesh@naresh-test-cluster.ap-south-1.eksctl.io --name naresh-test-cluster
````	
14) To check clusters list on argocd  
````
argocd cluster list
````

## ADD GIT REPOSITORY TO THE ARGOCD

--> Go to the argocd --> settings --> Repositories --> CONNECT REPO

Choose your connection method: VIA HTTPS

CONNECT REPO USING HTTPS:-

Type : git
Project : default
Repository URL : https://github.com/Velocity9919/Argo-CD-register-app-1

click on connect

## ADD APPLICATION ON ARGOCD 

--> go to the argocd --> applications --> NEW APP 

Application Name : register-app

Project Name : default

SYNC POLICY : Automatic

select both
PRUNE RESOURCES 
SELF HEAL 

SOURCE : -

Repository URL : select git url already witch you added

Revision : HEAD

Path : ./

DESTINATION : -

Cluster URL : select the cluster url witch you added
Namespace : default

click on create  ( it will sync and deploy application to the cluster )
````
kubectl get pods
````
````
kubectl get svc
````
````
a9f2354a42ed34d5383456843914754c-502301901.ap-south-1.elb.amazonaws.com:8080/webapp
````
