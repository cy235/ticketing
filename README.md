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

## Setup Kubernetes Cluster
We employ the DigitalOcean as our Iaas provider. First we need to create a Kubernetes cluster in DigitalOcean, then get the token from API token from DigitalOcean, which is named as doctl2 `7414c7733fb08f4559e3ec631973a2e87623380c99e2f2e61a030ad56131fa88` <br>

then initialize the doctl <br>

```
doctl auth init -t 7414c7733fb08f4559e3ec631973a2e87623380c99e2f2e61a030ad56131fa88 
```
connect the k8s cluster and save the name of Kubernetes cluster in the config file. <br>
```
doctl kubernetes cluster kubeconfig save ticketing 
```
Now you can observe the nodes in the k8s cluster <br>
```
$ kubectl get nodes
NAME                   STATUS   ROLES    AGE     VERSION
pool-1kexitqkv-3fel7   Ready    <none>   3m53s   v1.17.5
pool-1kexitqkv-3felc   Ready    <none>   3m52s   v1.17.5
pool-1kexitqkv-3felm   Ready    <none>   3m56s   v1.17.5
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

Now, we switched to context `do-nyc1-ticketing`, and create two secrets, json web token (JWT) and Stripe API keys, respectively

```
kubectl create secret generic jwt-secret  --from-literal=JWT_KEY=sadf78sfas8f76f86faf6
```
you can get your JWT by entering a random sequence. <br>
```
kubectl create secret generic stripe-secret --from-literal=STRIPE_KEY=YOURKEY
```
you can get Stripe API keys in this link: https://dashboard.stripe.com/test/dashboard.<br>

Also, we need to apply NGINX Ingress Controller in k8s cluster
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/do/deploy.yaml
```
and now you can find a load balancer is automatically created in DigitalOcean.

## CI/CD
In this project, we leverage **Github Actions** to build a CI/CD pipeline. <br>

You can update the `deploy-manifests.yaml`,`infra`, and dev branch files `auth`, `client`, `expiration`, `tickets`, `orders`, `payments` files respectively, and push them into dev branch, then create a pull request to merge the dev branch into the master branch, all the updated files will be integrated and push into the DockerHub, and deployed into k8s cluster automatically. When the images are first pushed in to the DockerHub, they are private, **CHANGE ALL THE PUSHED IMAGES IN THE DOCKERHUB INTO PUBLIC** <br>

You can modify any files in dev branch, make a pull request, it will fulfill the test automatically, and then merger the pull request, the updated files will be uploaded into DockerHub and deployed into k8s cluster automatically.  

If you only deploy images from DockerHub to k8s:
update the `deploy-manifests.yaml` and the `infra` files respectively, and push them into master branch.


If you want to delete all the deployments in k8s
```
kubectl delete -f infra/k8s && kubectl delete -f infra/k8s-prod
```
