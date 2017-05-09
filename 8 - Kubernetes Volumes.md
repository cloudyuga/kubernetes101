
 - Create a disk on GCE
```
❯ gcloud compute disks create --size=10GB mongodb-data
WARNING: You have selected a disk size of under [200GB]. This may result in poor I/O performance. For more information, see: https://developers.google.com/compute/docs/disks#pdperformance.
Created [https://www.googleapis.com/compute/v1/projects/cloudyuga-151310/zones/us-central1-a/disks/mongodb-data].
NAME          ZONE           SIZE_GB  TYPE         STATUS
mongodb-data  us-central1-a  10       pd-standard  READY

New disks are unformatted. You must format and mount a disk before it
can be used. You can find instructions on how to do this at:

https://cloud.google.com/compute/docs/disks/add-persistent-disk#formatting
```

Check it on Google cloud interface.

- Get into `volumes` directory for `rsvpapp`
```
$ cd volumes
```

- Create a `PersistentVolume` using GCE disk we created earlier. Make sure the name of disk (mongodb-data) matches with `pdName` inside `volume.yaml`. 

```
❯ kubectl create -f volume.yaml
persistentvolume "mongodb-pv" created

rsvpapp/kubernetes/volumes git/kubernetes*
❯ kubectl get PersistentVolume
NAME         CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS      CLAIM     STORAGECLASS   REASON    AGE
mongodb-pv   10Gi       RWO           Retain          Available                                      13s

rsvpapp/kubernetes/volumes git/kubernetes*
❯ kubectl describe PersistentVolume mongodb-pv
Name:		mongodb-pv
Labels:		<none>
Annotations:	<none>
StorageClass:
Status:		Available
Claim:
Reclaim Policy:	Retain
Access Modes:	RWO
Capacity:	10Gi
Message:
Source:
    Type:	GCEPersistentDisk (a Persistent Disk resource in Google Compute Engine)
    PDName:	mongodb-data
    FSType:	ext4
    Partition:	0
    ReadOnly:	false
Events:		<none>
```

- Claim the `PersistentVolume`, we created earlier 

```
❯ kubectl create -f rsvp-db_pv_claim.yaml
persistentvolumeclaim "mongodb-pv-claim" created

rsvpapp/kubernetes/volumes git/kubernetes*
❯ kubectl get PersistentVolumeClaim
NAME               STATUS    VOLUME       CAPACITY   ACCESSMODES   STORAGECLASS   AGE
mongodb-pv-claim   Bound     mongodb-pv   10Gi       RWO                          13s

rsvpapp/kubernetes/volumes git/kubernetes*
❯ kubectl describe PersistentVolume mongodb-pv
Name:		mongodb-pv
Labels:		<none>
Annotations:	pv.kubernetes.io/bound-by-controller=yes
StorageClass:
Status:		Bound
Claim:		nkhare/mongodb-pv-claim
Reclaim Policy:	Retain
Access Modes:	RWO
Capacity:	10Gi
Message:
Source:
    Type:	GCEPersistentDisk (a Persistent Disk resource in Google Compute Engine)
    PDName:	mongodb-data
    FSType:	ext4
    Partition:	0
    ReadOnly:	false
Events:		<none>
```

- Delete the `rsvp-db` deployment, if you already have 
```
❯ kubectl delete deployment rsvp-db
```

- Use the `PersistentVolumeClaim` inside a deployment
```
❯ kubectl create -f rsvp-db.yaml
```

- Create the service for `rsvp-db`, if you don't have it already
```
❯ kubectl create -f ../rsvp-db-service.yaml
```

- Create the frontend `rsvp` deployment and service if you don't have already
```
❯ kubectl create -f ../rsvp-web.yaml
❯ kubectl create -f ../rsvp-web-service.yaml
```

- Access the frontend and put some volues there

- Scale the backend `rsvp-db` to zero replicas 
```
❯ kubectl scale --replicas=0 -f rsvp-db.yaml
```

- Scale the backend `rsvp-db` to one replicas
``` 
❯ kubectl scale --replicas=0 -f rsvp-db.yaml
```

- Access the application and we should see the entries we did ealrier. 