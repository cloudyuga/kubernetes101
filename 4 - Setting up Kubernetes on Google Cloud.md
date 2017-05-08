- Setup an account on Google Cloud (cloud.google.com)
- Create a project if you don't have it already
- Setup [Google SDK on the laptop/workstation](https://cloud.google.com/sdk/downloads).
```
$ gcloud auth login
$ gcloud init
```
- Redeem the coupon 
    - Go to `cloud.google.com/redeem` to redeem the coupon, which is shared to you over email
- On Google cloud select `Container Engine`
- Click on `Create a container cluster`
    - Give a name to the container cluster (rootconf) 
    - Change machine type to Micro
    - Click on `Create`

- Click on `connect`, which would request to run command like following :-
```
$ gcloud container clusters get-credentials rootconf \
    --zone us-central1-a --project cloudyuga-151310
```

which would spit out output like following:-
```
Fetching cluster endpoint and auth data.
kubeconfig entry generated for rootconf.
```

- [Download `kubectl`](https://kubernetes.io/docs/tasks/kubectl/install/) if you have not already 
```
$ gcloud components install kubectl
```

- Run following command to test your connection with the `container cluster`
```
$ kubectl cluster-info
Kubernetes master is running at https://104.198.207.139
GLBCDefaultBackend is running at https://104.198.207.139/api/v1/proxy/namespaces/kube-system/services/default-http-backend
Heapster is running at https://104.198.207.139/api/v1/proxy/namespaces/kube-system/services/heapster
KubeDNS is running at https://104.198.207.139/api/v1/proxy/namespaces/kube-system/services/kube-dns
kubernetes-dashboard is running at https://104.198.207.139/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard
```

- Checkout the `~/.kube/config` file

- Create a namespace 
```
$ kubectl create namespace <ANYNAME>
```

- Get the current `context` 
```
$ kubectl config view | awk '/current-context/ {print $2}'
$ export CONTEXT=`kubectl config view | awk '/current-context/ {print $2}'` 
```

- Update the default namespace for the current context
```
$ kubectl config set-context $CONTEXT --namespace=<ANYNAME>
```

where `ANYNAME` is the name of the `namespace`, you created earlier. 

- Access the dashboard 
```
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```
 Open the browser with http://127.0.0.1:8001 URL. 
