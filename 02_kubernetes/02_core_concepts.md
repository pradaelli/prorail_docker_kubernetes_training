# Core Concepts

## Kubernetes objects

To see all the objects you can configure, run
```
kubectl api-resources
```

> ⚡️ All exercises below should be done in folder `02_core_concepts`

## Nodes

List the available nodes with:

```sh
kubectl get nodes
```

Now try

```sh
kubectl get nodes -o wide
```

Can you see the IP address of the nodes?

## Pod

Create your first pod in the file `my-pod.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: myapp-container
    image: busybox:stable
    command: ['sh','-c','echo Hello Kubernetes! && sleep 3600']
```

Take a look at this page https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/ for info on the Pod definition.

Apply the *manifest* you created with

```sh
kubectl apply -f my-pod.yaml
```

Run a few of the following commands:
```sh
kubectl get pods
kubectl logs my-pod
kubectl describe pod my-pod
kubectl get pods -o wide
```

*What do you see?*

### Editting a pod

Try `kubectl edit pod my-pod` This will create a `vi` session in which you can edit the running pod definition.

Compare the pod definition with the original `my-pod.yaml` 

Exit the session with "Esc" followed by typing `:q`

### Accessing a pod

Now run 

```
kubectl exec my-pod -- ls
```

And then

```
kubectl exec -it my-pod -- sh
```

You're now in the pod.  Try running `ls` to list files.

Type `exit` or push `Ctrl+D` to exit the pod.

## Namespaces

List the current namespaces with

```sh
kubectl get namespaces
```

To see the currently selected namespace, run

```sh
kubectl config get-contexts
```

You should see your current context identified with a `*`

Let's create a namespace

```sh
kubectl create namespace hello
```

To view it, run

```sh
kubectl get namespaces
```

To select it as default, run

```sh
kubectl config set-context --current --namespace=hello
```

Now check for pods.  Do you see any? 

Return to the default namespace as default with

```sh
kubectl config set-context --current --namespace=default
```

Again check for pods.

Now try this command:

```sh
kubectl get pods --all-namespaces
```



### Namespace with a file

Let's create a namespace with a file.  Create `namespace.yaml` with

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```

Apply the *manifest* with

```sh
kubectl apply -f namespace.yaml
```

Again check the list of namespaces.

*What would make using a file definition preferable to using the CLI to create namespaces?*

## Deploying to a Namespace

Now delete your created pod with

```sh
kubectl delete pod my-pod
```

Adjust `my-pod.yaml` to be as follows:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: my-namespace
spec:
  containers:
    - name: myapp-container
      image: busybox:stable
      command: ["sh", "-c", "echo Hello Kubernetes! && sleep 3600"]
```

Apply with

```sh
kubectl apply -f my-pod.yaml
```

Now retrieve a list of pods.  Do you see `my-pod`?

Retrieve this list of pods with

```sh
kubectl get pods -n my-namespace
```

Do you see your pod?

### Updating a pod

Instead of using `busybox:stable` change the reference to `busybox:latest` in `my-pod.yaml` 

Apply the manifest.  What happens? (use `describe` to see if your change was applied)

Now change the message for `command:` and again reapply.  What happens this time?

For more info: https://kubernetes.io/docs/concepts/workloads/pods/#pod-update-and-replacement

## Container Configuration

## restartPolicy

Create a new YAML file `one-shot-pod.yaml` containing:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: one-shot-pod
  namespace: my-namespace
spec:
  containers:
    - name: myapp-container
      image: busybox:stable
      command: ["sh", "-c", "echo Hello Kubernetes!"]
```

Apply the pod manifest.

Check the logs of the pod with `kubectl logs one-shot-pod -n my-namespace`

What do you see?

Now run 

```sh
kubectl describe pod one-shot-pod -n my-namespace
```

What do you see in the list of Events?

Delete the pod

Adjust `one-shot-pod.yaml` to be as follows:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: one-shot-pod
  namespace: my-namespace
spec:
  restartPolicy: Never
  containers:
    - name: myapp-container
      image: busybox:stable
      command: ["sh", "-c", "echo Hello Kubernetes!"]
```

Apply the manifest and again describe the pod.  What do you see?

### Connecting to a port

Apply the following:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: my-namespace
spec:
  containers:
    - name: nginx
      image: nginx:stable
```

Now run the following:

```sh
kubectl port-forward pod/nginx 8000:80
```

And go to http://localhost:8000 in your browser

Stop the port-forward once you are finished.
