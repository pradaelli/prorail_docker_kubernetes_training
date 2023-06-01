# Pod design

⚡️ All exercises done below should be done in folder `06_pod_design`

Create a new namespace and set it as default:

```sh
kubectl create namespace pod-design
kubectl config set-context --current --namespace=pod-design
```

## Labels

Create the file `label-pods.yaml` and apply:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-production-label-pod
  namespace: pod-design
  labels:
    app: my-app
    environment: production
spec:
  containers:
    - name: nginx
      image: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: my-development-label-pod
  namespace: pod-design
  labels:
    app: my-app
    environment: development
spec:
  containers:
    - name: nginx
      image: nginx
```

Use `describe` for one of the pods and look for the labels.

Try out a few of these commands:

```sh
kubectl get pods -l app=my-app
kubectl get pods -l environment=production
kubectl get pods -l environment=development
kubectl get pods -l environment!=production
kubectl get pods -l 'environment in (development,production)'
kubectl get pods -l app=my-app,environment=production
```

Can you see how labels could be useful when dealing with hundreds of pods? 

## Annotations

An example of Annotations, no need to apply

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-annotation-pod
  namespace: pod-design
  annotations:
    owner: myaccount@k8sisgr8.com
    git-commit: bdab0c6
spec:
  containers:
  - name: nginx
    image: nginx
```

## Deployments 

Create the file `single-pod.yaml` and apply:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: single-pod
  namespace: pod-design
spec:
  containers:
  - name: nginx
    image: nginx:stable
    ports:
       - containerPort: 80
```

Now create the file `nginx-deployment.yaml` containing:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: pod-design
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
        - containerPort: 80
```

Compare the deployment YAML with that of the pod.  What stands out?

Apply `nginx-deployment.yaml` and then `get` the pods.  What do you notice about the names?

Now open another terminal and run 

```sh
kubectl get pods -w
```

In your first terminal, delete one of the pods with `kubectl delete pod <the pod name>`

What happens?

Now run the following commands and observe what is returned:

```sh
# view the deployment
kubectl describe deploy nginx-deployment

# look at some replicasets
kubectl get replicaset

# show automatically generated labels with
k get pods --show-labels
```

## Rolling updates

Add the following to `rolling-deploy.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rolling-deployment
  namespace: pod-design
spec:
  strategy:
    rollingUpdate:
      maxSurge: 3
      maxUnavailable: 2
  replicas: 3
  selector:
    matchLabels:
      app: rolling
  template:
    metadata:
      labels:
        app: rolling
    spec:
      containers:
        - name: nginx
          image: nginx:1.7.1
          ports:
            - containerPort: 80
```

Apply the deployment manifest.

Open a second terminal.  If you have `watch` installed (check by running `watch` in the terminal) run:

```sh
watch kubectl get pods -l app=rolling
```

Otherwise use

```sh
kubectl get pods -l app=rolling -w
```

In your original terminal, run the following to change the image version

```sh
kubectl set image deployment/rolling-deployment nginx=nginx:1.7.9
```

and change the value of `image:` to `nginx:stable` 

Observe the second terminal and what happens.

> ⚠️ Note that in normal contexts, adjusting image versions using the CLI would be frowned upon.  Why do you think that might be?

### Rollouts and Rollbacks

View the rollout history by running

```sh
kubectl rollout history deployment/rolling-deployment
```

View the details of revision 2
```
kubectl rollout history deployment/rolling-deployment --revision=2
```

Roll back to revision 1:

```sh
kubectl rollout undo deployment/rolling-deployment
```

Note that you can rollback to a specific version by appending `--to-revision=<number>`

Check the rollout history again.  What do you see?

