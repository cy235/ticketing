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

update the `deploy-manifests.yaml`, `auth`, `client`, `expiration`, `tickets`, `orders`, `payments` files respectively push them into dev branch, then create a pull request to merge the dev branch into the master branch
