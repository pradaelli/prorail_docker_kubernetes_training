# Observability

⚡️ All exercises done below should be done in folder `05_observability`

Create a new namespace and set it as default:

```sh
kubectl create namespace observability
kubectl config set-context --current --namespace=observability
```



## Liveness probe

In `liveness-pod.yaml` add:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
  namespace: observability
spec:
  containers:
    - name: busybox
      image: busybox:stable
      command:
        [
          "sh",
          "-c",
          "touch /tmp/healthy; sleep 20; rm -f /tmp/healthy; sleep 600",
        ]
      livenessProbe:
        exec:
          command: ["cat", "/tmp/healthy"]
        initialDelaySeconds: 5
        periodSeconds: 5
```

And apply.

Watch the pod status with 

```
kubectl get pods -w
```

What happens after 20 seconds

Use `kubectl describe liveness-pod` to see what is happening.

## Readiness probe

An example of `readinessProbe`, no need to apply

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readiness-pod
  namespace: observability
spec:
  containers:
    - name: nginx
      image: nginx:1.20.1
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 5
```



## Metrics

Try out the following:

```sh
# see CPU and memory per pod
kubectl top pods

# specific pod
kubectl top pod my-resource-pod

# kube-system
kubectl top pods -n kube-system

# the nodes
kubectl top nodes
```

