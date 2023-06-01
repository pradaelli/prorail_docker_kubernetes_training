# Configuration

> ⚡️ All exercises done below should be done in folder `03_configuration`

Create a new namespace `my-configuration` and select it instead of `default`. 

## Environment variables

Create `my-pod-env.yaml` with

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod-env
  namespace: my-configuration
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox:stable
      command: ["sh", "-c", "echo $MESSAGE;"]
      env:
        - name: MESSAGE
          value: "Hello!"
```

Check the logs.

*What would be the downside of this if the value of `MESSAGE` had to be shared amongst multiple pods?*

## ConfigMap

Create the file `my-config-map.yaml` containing

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
  namespace: my-configuration
data:
  # key value style
  message: hello
  name: John

  # file style
  app.cfg: |
    key1=value1
    key2=value2

```

And apply.

Try out the following commands:

```sh
kubectl get configmap my-configmap -n my-configuration
kubectl describe configmap my-configmap -n my-configuration
```

Now let's use our ConfigMap

Create `my-pod-cf.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod-cf
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox:stable
      command: ["sh", "-c", "while true; do echo $MESSAGE $NAME; sleep 5; done;"]
      env:
        - name: MESSAGE
          valueFrom:
            configMapKeyRef:
              name: my-configmap
              key: message
        - name: NAME
          valueFrom:
            configMapKeyRef:
              name: my-configmap
              key: name
```

Now check the pod logs with

```sh
kubectl logs my-pod-cf -f # this will follow the logs
```

Try changing `hello` to `hi` in `my-config-map.yaml` and reapply (no delete necessary).  

*How does this affect the pod logs?*

### ConfigMap using volumes

Create the file `my-pod-cf-volumes.yaml` containing:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod-cf-volumes
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox:stable
      command: ["sh", "-c", "cat /config/app.cfg"]
      volumeMounts:
        - name: config
          mountPath: /config
          readOnly: true
  volumes:
    - name: config
      configMap:
        name: my-configmap
        items:
          - key: app.cfg
            path: app.cfg
```

Apply this and then check the logs of the new pod.

*If `app.cfg` was being loaded into an application for configuration of a database connection, would changing `app.cfg` impact the application?*

## Secrets 🤫

Add this to `my-secret.yaml`:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
stringData:
  secret1: icollectrocks
data:
  secret2: aWJyaW5ndGhlc3VudXB3aXRobXltaW5kCg==
---
apiVersion: v1
kind: Pod
metadata:
  name: my-pod-with-secrets
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox:stable
      command:
        [
          "sh",
          "-c",
          "while true; do echo $SECRET1 and $SECRET2; sleep 5; done;",
        ]
      env:
        - name: SECRET1
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: secret1
        - name: SECRET2
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: secret2
```

Apply the manifest and check the logs of the pod `my-pod-with-secrets`

