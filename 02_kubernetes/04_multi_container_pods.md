# Multi container pods

An example of using multi-container pods:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-test
spec:
  containers:
    - name: writer
      image: busybox:stable
      command:
        [
          "sh",
          "-c",
          'echo "The writer wrote this!" > /output/data.txt; while true; do sleep 5; done',
        ]
      volumeMounts:
        - name: shared
          mountPath: /output
    - name: sidecar
      image: busybox:stable
      command: ["sh", "-c", "while true; do cat /input/data.txt; sleep 5; done"]
      volumeMounts:
        - name: shared
          mountPath: /input
  volumes:
    - name: shared
      emptyDir: {}
```

Check the logs of the sidecar using:
```
kubectl logs sidecar-test -c sidecar
```

Writer outputs to `shared` which is mounted as `/output`, `sidecar` reads in from `shared` which is mounted as `/input`

