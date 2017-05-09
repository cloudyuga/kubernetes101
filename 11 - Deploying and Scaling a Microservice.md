
- Describe the [RSVP application](https://cloudyuga.gitbooks.io/container-orchestration/content/rsvp.html)

- Clone the `rsvpapp` git repo and checkout `kubernetets` branch
```
$ git clone https://github.com/cloudyuga/rsvpapp.git
$ git checkout kubernetes
$ cd kubernetes
```
- Explore the deployment and service configuration files for back end frontend. 

- Deploy the backend 
```
$ kubectl create -f rsvp-db.yaml
$ kubectl create -f rsvp-db-service.yaml
```

- Deploy the frontend
```
$ kubectl create -f rsvp-web.yaml
$ kubectl create -f rsvp-web-service.yaml
 ```

- List `Deployments` and `Services`
```
❯ kubectl get deployments
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
rsvp      2         2         2            2           20m
rsvp-db   1         1         1            1           21m

rsvpapp/kubernetes git/kubernetes
❯ kubectl get svc
NAME      CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
mongodb   10.87.249.186   <none>        27017/TCP      21m
rsvp      10.87.254.54    <nodes>       80:32505/TCP   17m
```

 - Open the `NodePort` and access the application from any of node's IP address. 
```
❯ kubectl get nodes -o yaml | grep -B 1 External
    - address: 104.197.224.162
      type: ExternalIP
--
    - address: 104.197.110.50
      type: ExternalIP
--
    - address: 104.197.130.144
      type: ExternalIP
```

- Pick up any node's IP address and access it on `NodePort`
```
$ open http://104.197.110.50:32505
```

- Scale the frontend
```
$ kubectl scale --replicas=3 -f rsvp-web.yaml
```

- Refresh the frontend and look changes on `Serving from Host` line
