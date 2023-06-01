# Volumes

⚡️ All exercises done below should be done in folder `08_volumes`

Create a new namespace and set it as default:

```sh
kubectl create namespace volumes-demo
kubectl config set-context --current --namespace=volumes-demo
```

## PersistentVolumeClaim

In the file `pvc.yaml` add:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-demo
  namespace: volumes-demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: standard-rwo
```

Apply this and run the following:

```sh
kubectl get persistenvolumeclaims
```

Note the value for `STATUS`. Let's find out more with

```sh
kubectl describe persistentvolumeclaims pvc-demo
```

Now check the PersistentVolume with

```sh
kubectl get persistentvolumes
```

Note the size and access mode.

In the Google Cloud Platform - Kubernetes Engine page, select "Storage". What do you see?

Let's have a Pod to use the PersistentVolumeClaim.

In `pod-pvc.yaml` add:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: pod-demo
  namespace: volumes-demo
spec:
  containers:
    - name: pod-demo
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - mountPath: "/data"
          name: pvc-demo-vol
  volumes:
    - name: pvc-demo-vol
      persistentVolumeClaim:
       claimName: pvc-demo
```

Use `get pods` to ensure the pod is running

Now run `kubectl get persistentvolumes` again.  What do you notice about the status.  Use `describe` to find out more.

Take a look the `persistentVolume` and "Storage" in GCP.
