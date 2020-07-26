# Minikube

## Creating Minikube

```
$ minikube start --kubernetes-version v1.15.12 --memory 4096
```

## Metallb

Enable metallb

```
$ minikube addons enable metallb
```

Get ip range

```
$ minikube ssh
$ cat /etc/hosts  // find 192.168.65.2	host.minikube.internal
$ minikube addons configure metallb
  - start 192.168.65.10
  - end   192.168.65.20
```
