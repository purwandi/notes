# Running Docker Image di Kubernetes

Tidak hanya menggunakan konfigurasi yaml, kita juga bisa menjalankan docker image secara langsung tanpa harus 
melalui proses pembuatan konfigurasi yaml terlebih dahulu. Sama seperti konsep penggunaan docker cli kita
bisa menjalan sebuah kontainer secara mudah

```
$ kubectl run [nama pod] --image nginx:alpine
$ kubectl run web-nginx --image nginx:alpine
```

Mari kita lihat di kubectl untuk pod yang sudah berhasil terbuat:

```sh
NAME            READY   STATUS    RESTARTS   AGE     IP          NODE             NOMINATED NODE   READINESS GATES
pod/web-nginx   1/1     Running   0          5m57s   10.1.0.35   docker-desktop   <none>           <none>
```

Dalam command `run` kita juga memberikan option untuk berinteraksi secara langsung

```
$ kubectl run [nama pod] -it --image nginx:alpine -- sh
```

Kita juga menambahkan command `--rm` sama seperti di docker, sehingga kita sudah selesai untuk melakukan
interaksi maka pod yang terbuat akan terhapus secara otomatis

```
$ kubectl run [nama pod] -it --rm --image nginx:alpine -- sh
```
