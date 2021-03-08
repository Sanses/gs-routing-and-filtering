# Pre-Conditions
- Deploy ingress-nginx
  - https://kubernetes.github.io/ingress-nginx/deploy/

# Azure Login and get AKS credentials
- az login
- az acr login -n ${ACR_NAME}
- az account set --subscription ${subscription_id}
- az aks get-credentials --resource-group ${AKS_RESOURCE_GROUP} --name ${AKS_CLUSTER_NAME} --admin

# Clone code
- git clone https://github.com/Sanses/gs-routing-and-filtering.git
- cd gs-routing-and-filtering
- git checkout ingress_and_gateway

# Deploy Book MicroService
- cd complete/book
- mvn clean package -Dmaven.test.skip=true
- docker build -t ${ACR_NAME}.azurecr.io/book:latest .
- docker push ${ACR_NAME}.azurecr.io/book:latest
- vi deployment.yml --> replace acrname
- kubectl create -f deployment.yml

# Deploy Gateway MicroService
- cd complete/gateway
- mvn clean package -Dmaven.test.skip=true
- docker build -t ${ACR_NAME}.azurecr.io/gateway:latest .
- docker push ${ACR_NAME}.azurecr.io/gateway:latest
- vi deployment.yml --> replace acrname
- kubectl create -f deployment.yml

# Create Ingress
- cd complete/k8s-ingress
- kubectl create -f ingress.yml
- kubectl get ingress

# Appendix
- https://kubernetes.github.io/ingress-nginx/deploy
- https://www.ibm.com/cloud/blog/spring-cloud-application-zuul-gateway-bluemix-kubernetes
- https://spring.io/guides/gs/routing-and-filtering/
- https://github.com/spring-guides/gs-routing-and-filtering
