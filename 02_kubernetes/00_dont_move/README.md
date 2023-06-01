# Kubernetes Training

We will be covering the following topics:

- Kubernetes core concepts
- Configuration
- Multi-container pods
- Observability
- Pod design
- Services and Networking
- State persistence
- Introduction to Helm

The setup requirements are as follows:

- a running instance of Docker, installation instructions [here](https://docs.docker.com/get-docker/)
- installation of kubectl, installation instructions [here](https://kubernetes.io/docs/tasks/tools/)
- nice to have, kubectx, installation instructions [here](https://github.com/ahmetb/kubectx#installation)
- a code editor like VS Code or IntelliJ
- git (for cloning the exercise code)


# Useful links:

- [Kubernetes documentation](https://kubernetes.io/docs/)
- [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [API reference](https://kubernetes.io/docs/reference/kubernetes-api/)
- [vi cheatsheet](https://www.atmos.albany.edu/daes/atmclasses/atm350/vi_cheat_sheet.pdf)

# Helpful commands

## kubectl

```sh

# create/update an object
kubectl apply -f <file>

# get objects
kubectl get <pods|nodes|service|..>
kubectl get <pods|nodes|service|..> --all-namespaces

# get details about an object
kubectl describe <nodes|pods|deployment> 

# show the pod logs
kubectl logs <pod_name>

```