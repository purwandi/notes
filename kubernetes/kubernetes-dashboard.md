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

Untuk membuat koneksi proxy dapat dilakukan perintah sebagai berikut:

```bash
$ kubectl proxy
```

Nah sekarang anda bisa melakukan akses ke kubernetes dashboard pada 

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

*Akses sewaktu-waktu dapat mengalami perubahan, selalu update ke sumber*