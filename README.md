# ticketing_app\

`kubectl config view`

you can switch into local k8s cluster: 
`kubectl config use-context docker-desktop` 
or into digital ocean k8s cluster:
`kubectl config use-context do-nyc1-ticketing-app`
Switched to context "do-nyc1-ticketing-app".
`kubectl create secret generic jwt-secret  --from-literal=JWT_KEY=sadf78sfas8f76f86faf6`

