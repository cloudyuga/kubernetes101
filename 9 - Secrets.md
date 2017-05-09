
- Create a file with a password and make sure there is no trailing new line

```
❯ echo "rootconf" > password.txt
❯ tr -Ccsu '\n' <password.txt >.strippedpassword.txt && mv .strippedpassword.txt password.txt
```

- Create a secret 
```
❯ kubectl create secret generic my-password --from-file=password.txt
❯ kubectl get secret my-password
NAME          TYPE      DATA      AGE
my-password   Opaque    1         12s

kubernetes101/secret/mysql-wordpress git/master*
❯ kubectl describe secret my-password
Name:		my-password
Namespace:	default
Labels:		<none>
Annotations:	<none>

Type:	Opaque

Data
====
password.txt:	8 bytes
```

- Create a pod and access the secret from it. 
```
❯ cat pod.yam
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
    - name: mycontainer
      image: nginx:alpine
      env:
        - name: SECRET_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-password
              key: password.txt
  restartPolicy: Never


 ❯  kubectl create -f pod.yaml
  ```

- Access the pod and check environment variable.

```
kubectl exec -it secret-env-pod sh
/ # env
KUBERNETES_PORT=tcp://10.3.240.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=secret-env-pod
SHLVL=1
HOME=/root
SECRET_PASSWORD=rotconf

NGINX_VERSION=1.13.0
KUBERNETES_PORT_443_TCP_ADDR=10.3.240.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.3.240.1:443
PWD=/
KUBERNETES_SERVICE_HOST=10.3.240.1
```