# Helm

## Create Service Account dan Cluster Role Binding

Sebelum dapat menggunakan helm akan lebih kalau kita membuat service account baru untuk tools helm.
Pembuatan service account akan kita letakkan di namespace `kube-system`

```sh
$ kubectl -n kube-system create serviceaccount tiller
```

```sh
$ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
```

## Install Tiller

```sh
$ kubectl init --serviceaccount tiller
```
