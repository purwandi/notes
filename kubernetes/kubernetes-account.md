# Create Service Account

```sh
kubectl -n kube-system create serviceaccount tiller
```

# Attach Cluster Role Binding

```sh
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
```

# Helm init with service account

```sh
helm init --service-account tiller
```
