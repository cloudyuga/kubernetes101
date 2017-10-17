## Services

![](https://kubernetes.io/images/docs/services-iptables-overview.svg)

- ClusterIP

### Accessing the application from external world
#### ExternalIP
#### NodePort
#### Load Balancer

```
$ kubectl expose deployment my-nginx --port=80 --type=NodePort

❯ kubectl expose deployment my-nginx --port=80 --type=NodePort
service "my-nginx" exposed

~   6s
❯ kubectl get svc
NAME       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
my-nginx   10.87.245.182   <nodes>       80:31581/TCP   6s

~
❯ kubectl describe svc my-nginx
Name:			    my-nginx
Namespace:		    nkhare
Labels:			    run=my-nginx
Annotations:	    <none>
Selector:		    run=my-nginx
Type:			    NodePort
IP:			        10.87.245.182
Port:			    <unset>	80/TCP
NodePort:		    <unset>	31581/TCP
Endpoints:		    10.84.1.9:80,10.84.2.6:80
Session Affinity:	None
Events:			    <none>
```

- Goto the Google Cloud Console and create `Networking -> Firewall rules` to open up port mentioned in `NodePort`. In my case it is `31581`. 
    - Create a firewall rule
    - Name -> <Some Name> 
    - Source IP ranges -> 0.0.0.0/0
    - Protocols and ports  -> Specified protocols and ports -> tcp:31581

- Get the Node's external IP address. 

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
$ open http://104.197.110.50:31581
```

- Equivalent Service configuration file. 
```
❯ cat my-nginx-svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```

- Delete the service
```
❯ kubectl delete svc my-nginx
service "my-nginx" deleted
```

- create a service with following configuration file
```
❯ cat my-nginx-sv-lb.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```

```
$ kubectl crate -f  my-nginx-sv-lb.yaml
```

```
❯ kubectl get svc
NAME       CLUSTER-IP      EXTERNAL-IP       PORT(S)        AGE
my-nginx   10.87.249.111   104.154.252.253   80:30122/TCP   1m
```

Access the application with `EXTERNAL-IP`
