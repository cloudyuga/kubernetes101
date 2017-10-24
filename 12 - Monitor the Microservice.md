- Setup Prometheus
```
$ cd rsvpapp/kubernetes/prometheus
$ kubectl create -f prometheus-configmap.yaml
$ kubectl create -f prometheus-controller.yaml
$ kubectl create -f prometheus-service.yaml
```

- List the services and get the extrnal IP address for the `prometheus` service
```
$  kubectl get svc
NAME         CLUSTER-IP     EXTERNAL-IP       PORT(S)        AGE
kubernetes   10.3.240.1     <none>            443/TCP        23h
mongodb      10.3.247.36    <none>            27017/TCP      4h
prometheus   10.3.253.103   104.154.199.189   80:30125/TCP   4h
```

- Access `prometheus` using external IP address.

- Create the backed `rsvp-db` deployment and service if you don't have already
```
❯ kubectl create -f ../rsvp-db.yaml
❯ kubectl create -f ../rsvp-db-service.yaml
```

- Create the deployment for frontend `rsvp` which exports it matrices to `Prometheus`

```
❯ cat rsvp-web-prom.yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: rsvp
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: rsvp
        name: rsvp
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "8000"
        prometheus.io/path: ""
    spec:
      containers:
      - name: rsvp-app
        image: teamcloudyuga/rsvpapp:prometheus
        env:
        - name: MONGODB_HOST
          value: mongodb
        ports:
        - containerPort: 5000
          name: web-port
        - containerPort: 8000

❯ kubectl create -f rsvp-web-prom.yaml
```

- Deploy the `rsvp` service with `LoadBalancer` configuration

- Access the `rsvp` application and perform some operations.

- Open the `Prometheus` GUI and search for `flask_request_count` expression

- Install `ab` tool on the workstation create a `json` file with following content :-

```
$ cat entry.json
{"name": "test", "email": "test@example.com"}
```

- Get the external IP address of `rsvp` service

- Run the `ab` tool with following option

```
$ ab -p entry.json -T application/json -c 10 -n 10 http://<External IP Address RSVP>/api/rsvps
```

- Go the `Prometheus` GUI and search
 -  `flask_request_count`
 - `flask_request_count{endpoint="/api/rsvps"}`
