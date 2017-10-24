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

- Change to `configmaps` folder
```
❯ cd configmaps
```

- Deploy the `customer1-configmap.yaml` `ConfigMap`
```
❯ cat customer1-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: customer1
data:
  TEXT1: Customer1_Company
  TEXT2: Welcomes You
  COMPANY: Customer1 Company Technology Pvt. Ltd.

rsvpapp/kubernetes/configmaps git/kubernetes
❯ kubectl create -f customer1-configmap.yaml
configmap "customer1" created
```

- Delete the existing `rsvp` deployment if you have
```
❯ kubectl delete deployment rsvp
deployment "rsvp" deleted
```

- Create the `deployment` from the `configmaps` folder
```
❯ kubectl create -f rsvp-web-customer1.yaml
deployment "rsvp" created
```

- Create the service for `rsvp` frontend, if you don't have it already 
```
❯ kubectl create -f ../rsvp-web-service.yaml
```

- Are you able access the application ? Why not ? 

