# Build Ticketing Web Application with Microservices 
## Overview
In this project, we build a ticket ordering web application with React, Node, Express, and MongoDB, which consisting of 6 microservices. We leverage Github to continuously integrate the web application images and delivered them into the Docker Hub, then continuously deployed them into the Kubernetes cluster. The following figure shows the development workflow
<p align="center">
  <img width="460" src="https://github.com/cy235/ticketing/blob/master/images/local_git.jpg">
</p>


This web application consits of 6 microservices, i.e., `orders`,`tickets`,`payments`,`client`, `expiration` (the ordering will be expired if the information is not filled within the given time), and `auth`(authentication), which are shown in the following figure
<p align="center">
  <img width="200" src="https://github.com/cy235/ticketing/blob/master/images/micro_service.jpg">
</p>


The deployment plan is shown in the following figure
<p align="center">
  <img width="660" src="https://github.com/cy235/ticketing/blob/master/images/microservice_chart%20(1).jpg">
</p>

where each single micro service can be continuously built and deployed, the infra file is responsible for deploying the whole applcation (including all micro services). 

## Setup
We employ the DigitalOcean as our Iaas provider. First we need to create a Kubernetes cluster in DigitalOcean, then get the token from API token from DigitalOcean, which is named as doctl2 `a0ca66ad811be78ebaaa2b831d6619f8e12a396c741a0907dd4f6ed12cac1682` <br>

then initialize the doctl <br>

```
doctl auth init -t a0ca66ad811be78ebaaa2b831d6619f8e12a396c741a0907dd4f6ed12cac1682 
```
save the name of Kubernetes cluster in the config file <br>
```
doctl kubernetes cluster kubeconfig save ticketing 
```

Then go to github and add the `DOCKER_USERNAME` and `DOCKER_PASSWORD` in the secretes
add `DIGITALOCEAN_ACCESS_TOKEN` in the secretes,
where `DIGITALOCEAN_ACCESS_TOKEN` value is created under the DigitalOcean API token named `github_access_token`.

We can observe the available K8s clusters 
`kubectl config view`

We can switch into local k8s cluster: 
```
kubectl config use-context docker-desktop
``` 
or into DigitalOcean k8s cluster:
```
kubectl config use-context do-nyc1-ticketing
```

Now, we switched to context `do-nyc1-ticketing`, and create two secrets, json web token and stripe API keys, respectively


```
kubectl create secret generic jwt-secret  --from-literal=JWT_KEY=sadf78sfas8f76f86faf6
```

```
kubectl create secret generic stripe-secret --from-literal=STRIPE_KEY=YOURKEY
```
Also, we need to apply NGINX Ingress Controller in k8s cluster
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/do/deploy.yaml
```

## CI/CD
Initialization of CI/CD pipeline: 
* update the `deploy-manifests.yaml`,`infra`, and dev branch files `auth`, `client`, `expiration`, `tickets`, `orders`, `payments` files respectively, and push them into dev branch, then create a pull request to merge the dev branch into the master branch

**MAKE SURE ALL THE PUSHED IMAGES IN THE DOCKERHUB ARE PUBLIC**

You can modify any files in dev branch, make a pull request, it will fulfill the test automatically, and then merger the pull request, the updated files will be uploaded into DockerHub and deployed into k8s cluster automatically.  

If deploy images from DockerHub to k8s only:
update the `deploy-manifests.yaml` and the `infra` files respectively, and push them into master branch


If you want to delete all the deployments in k8s
```
kubectl delete -f infra/k8s && kubectl delete -f infra/k8s-prod
```
