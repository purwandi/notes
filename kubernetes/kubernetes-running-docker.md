# Running Docker Image di Kubernetes

Tidak hanya menggunakan konfigurasi yaml, kita juga bisa menjalankan docker image secara langsung tanpa harus 
melalui proses pembuatan konfigurasi yaml terlebih dahulu. Sama seperti konsep penggunaan docker cli kita
bisa menjalan sebuah kontainer secara mudah

## Penggunaan `kubectl run`

Cara pertama menggunakan `kubectl run`. Penggunaan perintah ini digunakan untuk membuat dan menjalankan 
sebuah docker image dalam sebuah pod. 

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

Penggunaan run di atas **hanya** akan membuat pod, bukan membuat sebuah deployment

## Penggunaan `kubectl create deployment`

Kita juga bisa menggunakan `kubectl create deployment` untuk membuat sebuah deployment dari app

```
$ kubectl create deployment [nama deployment] --image=[nama docker image]
$ kubectl create deployment nginx-proxy --image=nginx:alpine
```

Mari kita lihat di kubectl untuk pod yang sudah berhasil terbuat:

```sh
NAME                               READY   STATUS    RESTARTS   AGE   IP          NODE             NOMINATED NODE   READINESS GATES
pod/nginx-proxy-5c5bf79d45-7p2b4   1/1     Running   0          37s   10.1.0.37   docker-desktop   <none>           <none>

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-proxy   1/1     1            1           37s   nginx        nginx:alpine   app=nginx-proxy

NAME                                     DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         SELECTOR
replicaset.apps/nginx-proxy-5c5bf79d45   1         1         1       37s   nginx        nginx:alpine   app=nginx-proxy,pod-template-hash=5c5bf79d45
```

## Scaling

Untuk melakukan scaling pada deployment yang tadi kita lakukan kita dapat menggunakan perintah `kubectl scale`,  berikut ini
contoh penggunaan scaling 

```
$ kubectl scale [nama deployment] --replicas [jumlah replica]
// scale up
$ kubectl scale deployment.apps/nginx-proxy --replicas 10
// scale down
$ kubectl scale deployment.apps/nginx-proxy --replicas 1
```


## Mengakses Pod Service

Ada beberapa cara untuk mengakses port service

### kubectl port-forward

Kita tahu bahwa nginx docker akan berjalan di antar port 80 atau 444, untuk mengakses service tersebut kita bisa menggunakan
port-forward. Perintah ini akan melakukan forwarding locar port ke sebuah pod. 

```sh
// single port forward
$ kubectl port-forward [nama pod] [port local]:[port pod]
$ kubectl port-forward nginx-proxy-5c5bf79d45-7p2b4 8080:80

// multiple port forward
$ kubectl port-forward [nama pod] [port local]:[port pod] [port local]:[port pod]
$ kubectl port-forward nginx-proxy-5c5bf79d45-7p2b4 8080:80 8081:443
```

Sekarang coba di cek di http://localhost:80 browser anda, maka kita sudah mendapatkan akses p

### kubectl expose 

Cara port-forwarding di atas akan sangat sulit dilakukan apabila kita memiliki 2 atau lebih pod yang akan akan kita expose.
Cara lain adalah dengan menggunakan service dengan command `kubectl expose`

```sh
$ kubectl expose deployment [deployment name] --port [container port] --type [service type]
// type bisa berisi ClusterIP, NodePort, LoadBalancer
$ kubectl expose deployment nginx-proxy --port 80 --type NodePort
```

## Mendapatkan Konfigurasi Yaml

```
$ kubectl get deployment ngix-proxy -o yaml
$ kubectl get service ngix-proxy -o yaml
```

## Mengapus Pod

Untuk pod yang sudah dibuat kita bisa melakukan penghapusan dengan menggunakan perintah `kubectl delete pod [pod name]`

```
$ kubectl delete pod web-nginx
$ kubectl delete pod/web-nginx
```

## Menghapus Deployment

```
$ kubectl delete deployment nginx-proxy
$ kubectl delete deployment.apps/nginx-proxy
```

## Menghapus Service

```
$ kubectl delete service nginx-proxy
$ kubectl delete service/nginx-proxy
```

## Note

Tulisan yang terdapat pada tulisan ini tidak dianjurkan dalam stage production, gunakanlah konfigurasi yaml dan simpan
dalam sebuah git repository sehingga perubahan dari konfigurasi tersebut bisa ditrack

