# Kubernetes Dashboard

Merupakan service yang ditujukan untuk memvisualisasikan kubernetes service
melalui browser UI based

[Kubernetes Dashboard](https://github.com/kubernetes/dashboard/)

## Instalasi

Instalasi secara update terdapat pada link preferensi di atas, atau overview
proses intalasi terdapat pada catatan dibawah ini:


Untuk melakukan deploy kubernetes dashboard

```bash
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.1/aio/deploy/recommended.yaml
```

*Versi v2.0.1 kemungkinan bisa berubah sewaktu-waktu, selalu update ke sumber* 

## Akses

Untuk melakukan akses terhadap kubernetes dashboard tidak dapat dilakukan secara
langsung melalui kubernetes cluster, melainkan dengan melakukan pembuatan proxy
dari komputer lokal (kubectl) client

### Proxy

Untuk membuat koneksi proxy dapat dilakukan perintah sebagai berikut:

```bash
$ kubectl proxy
```

### Akses UI

Nah sekarang anda bisa melakukan akses ke kubernetes dashboard pada 

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

*Akses sewaktu-waktu dapat mengalami perubahan, selalu update ke sumber*

### Token

Untuk mendapatkan token yang valid agar bisa masuk ke dalam kubernetes dashboard
kita harus mencari token secret, untuk melakukan hal itu kita mencari secret
dengan nama `deployment` atau `deployer`

Apabila kita belum yakin terhadap nama secret tersebut kita dapat memprint
seluruh secret yang ada pada kubernetes cluster dengan cara

```
$ kubectl -n kube-system get secret
```

Maka output yang dihasilkan bisa seperti ini:

```bash
cronjob-controller-token-nvwrj                   kubernetes.io/service-account-token   3      117m
daemon-set-controller-token-r2nff                kubernetes.io/service-account-token   3      117m
default-token-6pspn                              kubernetes.io/service-account-token   3      116m
deployment-controller-token-fxsph                kubernetes.io/service-account-token   3      117m
disruption-controller-token-v7tg5                kubernetes.io/service-account-token   3      117m
endpoint-controller-token-4h89f                  kubernetes.io/service-account-token   3      117m
expand-controller-token-h9lpj                    kubernetes.io/service-account-token   3      117m
generic-garbage-collector-token-s2v5k            kubernetes.io/service-account-token   3      117m
horizontal-pod-autoscaler-token-jrhdh            kubernetes.io/service-account-token   3      117m
```

Kita sudah mengetahui `deployment-controller-token-fxsph` adalah secret yang 
nanti kita akan pakai. Untuk mendapatkan nilai token secret tersebut

```
$ kubectl -n kube-system describe secret deployment-controller-token-fxsph
```

Salin token yang tampil dan masukkan kedalam field UI kubernetes dashboard 
untuk bisa melakukan akses.

Cara di atas merupakan cara yang sebenarnya harus dilakukan untuk mendapatkan 
token secret untuk melakukan akses ke kubernetes. Namun ada cara yang lebih
singkat untuk mendapatkan token dengan perintah berikut:

```
$ kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | awk '/^deployment-controller-token-/{print $1}') | awk '$1=="token:"{print $2}'
```