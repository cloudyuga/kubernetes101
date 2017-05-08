

$ kubectl run my-nginx --image=nginx:alpine --replicas=2 --port=80

❯ kubectl get deployments
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
my-nginx   2         2         2            2           31s

❯ kubectl get replicasets
NAME                DESIRED   CURRENT   READY     AGE
my-nginx-64405743   2         2         2         51s

kubernetes101 git/master*
❯ kubectl get pods
NAME                      READY     STATUS    RESTARTS   AGE
my-nginx-64405743-hffj1   1/1       Running   0          57s
my-nginx-64405743-sf1mb   1/1       Running   0          57s

❯ kubectl get pods -L run
NAME                      READY     STATUS    RESTARTS   AGE       RUN
my-nginx-64405743-hffj1   1/1       Running   0          4m        my-nginx
my-nginx-64405743-sf1mb   1/1       Running   0          4m        my-nginx

❯ kubectl get pods -L run,run1
NAME                      READY     STATUS    RESTARTS   AGE       RUN        RUN1
my-nginx-64405743-hffj1   1/1       Running   0          4m        my-nginx   <none>
my-nginx-64405743-sf1mb   1/1       Running   0          4m        my-nginx   <none>

❯ kubectl get pods  --selector="run==my-nginx"
NAME                      READY     STATUS    RESTARTS   AGE
my-nginx-64405743-hffj1   1/1       Running   0          2h
my-nginx-64405743-sf1mb   1/1       Running   0          2h

❯ kubectl edit my-nginx-64405743-hffj1
the server doesn't have a resource type "my-nginx-64405743-hffj1"

❯ kubectl get pods  -l "env==dev"
NAME                      READY     STATUS    RESTARTS   AGE
my-nginx-64405743-hffj1   1/1       Running   0          2h