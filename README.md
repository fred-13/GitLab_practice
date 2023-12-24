# Setup gitlab on minikube using helm3

## Start minikube with virtualbox driver
```
minikube start --driver kvm2 --cpus 4 --memory 10g
```

**Note:** Other drivers are listed here https://minikube.sigs.k8s.io/docs/drivers/
Some times your pods will not be able start due to permission issue when using other drivers.

## Enable few minikube addons
```
minikube addons enable ingress
minikube addons enable dashboard
```
Read more: https://minikube.sigs.k8s.io/docs/commands/addons/

## Update Gitlab Helm Chart
```
helm repo update
```

## Install GitLab in minikube
```
helm upgrade --install gitlab . \
    --timeout 600s \
    -f ./values-minikube.yaml \
    --set global.hosts.domain=$(minikube ip).nip.io \
    --set global.hosts.externalIP=$(minikube ip)
```

## Watch pods
```
watch kubectl get pods
```
**note:** till all the pods are not up and running, you won't be able to access the private gitlab. Sometimes it takes 15-20 mins and you might see some failures as well, its ok wait for some time.

## Get minikube ip
```
minikube ip
```
and open something like below in a browser
```
https://gitlab.192.168.99.100.nip.io/
```

## Get the password
```
kubectl get secret gitlab-gitlab-initial-root-password -ojsonpath='{.data.password}' | base64 --decode ; echo
```

**Source:** https://docs.gitlab.com/charts/development/minikube/
