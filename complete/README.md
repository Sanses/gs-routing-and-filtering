# Pre-Conditions
- Deploy ingress-nginx
  - https://kubernetes.github.io/ingress-nginx/deploy/

# Azure Login and get AKS credentials
- az login
- az acr login -n sansaeacr
- az account set --subscription ${subscription_id}
- az aks get-credentials --resource-group sansae-demo-AKS-RG --name sansaeaks --admin

# Clone code
- git clone https://github.com/Sanses/gs-routing-and-filtering.git
- cd gs-routing-and-filtering
- git checkout ingress_and_gateway

# Deploy Book MicroService
- cd complete/book
- mvn clean package -Dmaven.test.skip=true
- docker build -t sansaeacr.azurecr.io/book:latest .
- docker push sansaeacr.azurecr.io/book:latest
- kubectl create -f deployment.yml

# Deploy Gateway MicroService
- cd complete/gateway
- mvn clean package -Dmaven.test.skip=true
- docker build -t sansaeacr.azurecr.io/gateway:latest .
- docker push sansaeacr.azurecr.io/gateway:latest
- kubectl create -f deployment.yml

# Create Ingress
- cd complete/k8s-ingress
- kubectl create -f ingress.yml
- kubectl get ingress
