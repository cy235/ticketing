# ticketing
get the token from API token from DigitalOcean, which is named as doctl2
`a0ca66ad811be78ebaaa2b831d6619f8e12a396c741a0907dd4f6ed12cac1682`
then
```
doctl auth init -t a0ca66ad811be78ebaaa2b831d6619f8e12a396c741a0907dd4f6ed12cac1682
```

```
doctl kubernetes cluster kubeconfig save ticketing
```


then go to github and add the DOCKER_USERNAME and DOCKER_PASSWORD in the secretes
add DIGITALOCEAN_ACCESS_TOKEN in the secretes
where DIGITALOCEAN_ACCESS_TOKEN value is created under the DigitalOcean API token named `github_access_token`

`kubectl config view`

you can switch into local k8s cluster: 
```
kubectl config use-context docker-desktop
``` 
or into digital ocean k8s cluster:
```
kubectl config use-context do-nyc1-ticketing
```
Switched to context "do-nyc1-ticketing".



```
kubectl create secret generic jwt-secret  --from-literal=JWT_KEY=sadf78sfas8f76f86faf6
```

```
kubectl create secret generic stripe-secret --from-literal=STRIPE_KEY=YOURKEY
```

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/do/deploy.yaml
```
Initialization of CI/CD pipeline: 
* update the `deploy-manifests.yaml`,`infra`, and dev branch files `auth`, `client`, `expiration`, `tickets`, `orders`, `payments` files respectively, and push them into dev branch, then create a pull request to merge the dev branch into the master branch

MAKE SURE ALL THE PUSHED IMAGES IN THE DOCKERHUB ARE PUBLIC

You can modify any files in dev branch, make a pull request, it will fulfill the test automatically, and then merger the pull request, the updated files will be uploaded into DockerHub and deployed into k8s cluster automatically.  

If deploy images from DockerHub to k8s only:
update the `deploy-manifests.yaml` and the `infra` files respectively, and push them into master branch


If you want to delete all the deployments in k8s
```
kubectl delete -f infra/k8s && kubectl delete -f infra/k8s-prod
```
