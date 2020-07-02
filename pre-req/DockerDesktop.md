# Docker Desktop | Our Playground

Simple setup for Docker and Kubernetes. Also prehaps the fastest way to containerize applications on your desktop.

## Why Docker Desktop

I choose Docker Desktop for following compelling reasons

- Saves us from the need to fiddle with VMs or add a bunch of extra components
- One click Kuberenetes reset - which btw will come handy while we try different Istio features. 
- How do you do that: Go to `Docker Desktop > Preferences > Kubernetes > Reset Kubernetes Cluster`.

## Dashboard UI

The Dashboard UI is not deployed by default. To deploy it, run the following command:

```yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
kubectl proxy
```

> Kubernetes Dashboard is running now on <http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/>

### Accessing Dashboard UI

We will find out how to create a new user using Service Account mechanism of Kubernetes, grant this user admin permissions and login to Dashboard using bearer token tied to this user.

### Creating a ServiceAccount

We are creating Service Account with name admin-user in namespace kubernetes-dashboard first.

```yaml
kubectl apply -f kube-user/admin-user.yaml
```

### Creating a ClusterRoleBinding

In most cases after provisioning cluster using `kops`, `kubeadm` or any other popular tool, the ClusterRole cluster-admin already exists in the cluster. We can use it and create only `ClusterRoleBinding` for our ServiceAccount. If it does not exist then you need to create this role first and grant required privileges manually using below command.

```yaml
kubectl apply -f user/cluster-admin-role.yaml
```

## Granting Access Token

```yaml
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

Storing the token for reuseability purposes

```yaml
TOKEN=$(kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}') | grep 'token:' | awk '{print $2}')
echo $TOKEN
```

Now we can use `token` to login the Kubernetes Dashboard.

## References

- [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
- [Granting dashboard access token](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md)
