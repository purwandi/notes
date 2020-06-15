# Create kubernetes yaml file easy way

Membuat konfigurasi file yaml di kubernetes sangat susah dan banyak sering kali kita tidak menghafal sintaksnya. 
Ada cara lebih mudah untuk membuat konfigurasi file tersebut dengan cara `kubectl`. 

Pada `kubectl` terdapat opsi untuk print output dari command yang akan dilakukan. 

Semisal pada perintah dibawah ini adalah untuk membuat deployment dengan nama nginx proxy dan image yang digunakan 
adalah nginx:alpine.

## Deployment

```
$ kubectl create deployment nginx-proxy --image=nginx:alpine
```

Maka untuk membuat konfigurasi yamlnya bisa menambahkan output yaml di command yang dilakukan

```
$ kubectl create deployment nginx-proxy --image=nginx:alpine -o yaml --dry-run
```

kita menggunakan `--dry-run` agar command tersebut tidak dikirim ke kubernetes cluster sehingga proses deployment tidak
akan dilakukan. Perintah di atas akan menghasil yaml file sebagai berikut:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx-proxy
  name: nginx-proxy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-proxy
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx-proxy
    spec:
      containers:
      - image: nginx:alpine
        name: nginx
        resources: {}
status: {}
```

nah dari output tersebut kita tinggal melakukan edit beberapa bagian saja :-)


## Service

Output ini juga terdapat pada service, berikut ini adalah perintah untuk melakukan pembuat service yang mengekspose
deployment nginx-proxy dimana deployment tersebut terdapat container yang melakukan serving di port 80 dan 443

```
$ kubectl expose deployment nginx-proxy --port 80,443 --type NodePort -o yaml --dry-run --type NodePort
```

Perintah di atas akan menghasilkan file yaml sebagai berikut:

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx-proxy
  name: nginx-proxy
spec:
  ports:
  - name: port-1
    port: 80
    protocol: TCP
    targetPort: 80
  - name: port-2
    port: 443
    protocol: TCP
    targetPort: 443
  selector:
    app: nginx-proxy
  type: NodePort
status:
  loadBalancer: {}
```

Edit konfigurasi dengan seperlunya lalu bisa di save :-)
