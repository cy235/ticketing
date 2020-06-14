# ticketing_app\
get the token from API token from DigitalOcean, which is named as doctl2
`a0ca66ad811be78ebaaa2b831d6619f8e12a396c741a0907dd4f6ed12cac1682`
then
`doctl auth init -t a0ca66ad811be78ebaaa2b831d6619f8e12a396c741a0907dd4f6ed12cac1682`

`kubectl config view`

you can switch into local k8s cluster: 
`kubectl config use-context docker-desktop` 
or into digital ocean k8s cluster:
`kubectl config use-context do-nyc1-ticketing-app`
Switched to context "do-nyc1-ticketing-app".
`kubectl create secret generic jwt-secret  --from-literal=JWT_KEY=sadf78sfas8f76f86faf6`

