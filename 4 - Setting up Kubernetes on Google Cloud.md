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
kubectl get nodes
NAME                                      STATUS    AGE       VERSION
gke-rootconf-default-pool-f1903f03-40c6   Ready     14m       v1.5.7
gke-rootconf-default-pool-f1903f03-c8j0   Ready     14m       v1.5.7
gke-rootconf-default-pool-f1903f03-nlgs   Ready     14m       v1.5.7
```
