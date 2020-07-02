# Istio: Getting Running

Guide to help you start with Istio

Reference: <https://istio.io/docs/setup/getting-started/#download>

## 1. Download Istio

```bash
curl -L https://istio.io/downloadIstio | sh -
cd istio-1.6.0
export PATH=$PWD/bin:$PATH
```

The command above downloads the latest release (numerically) of Istio. To download a specific version, you can add a variable on the command line. For example to download Istio 1.4.3, you would run

`curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.4.3 sh -`

## 2. Install Istio

We do use either of two ways to install Istio

### 2.1.A. Using istioctl

```bash
istioctl install --set profile=demo
```

> ✔ Istio core installed
> ✔ Istiod installed
> ✔ Egress gateways installed
> ✔ Ingress gateways installed
> ✔ Addons installed
> ✔ Installation complete

### 2.1.B. Using custom resource definitions

We can also manually install i.e. extend Kubernetes capabilities for Istio usning custom resource definitions - [istio-crds.yaml](./istio-crds.yaml)

Please note that we haven't yet enabled Istio auto injection.

```bash
kubectl apply -f istio-crds.yaml
```

Install demo configuration ([istio-demo.yaml](./istio-demo.yaml)):

```bash
kubectl apply -f istio-demo.yaml
```

### 2.2. Verify Installation

Running objects:

```bash
kubectl get all -n istio-system
```

You must see pods, services, deployments etc. running inside `istio-system` namespace.

## 3. Enable Envoy proxy injection

Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later.

```bash
kubectl label namespace default istio-injection=enabled
```

> ✔ namespace/default labeled

Check label:

```bash
kubectl describe namespace default
```

## Check what's running

```bash
kubectl get all -n istio-system

docker info
```

> **Istio is expensive!!!**

**Containers running: MANY.** And upto this point, we aren't even using Istio for our needs. It's a lot of compute power, and memory usage.

Well, if that concerns you, here's an article which explains more on cost front, and it's comparison with other options we've in market

- [Benchmarking Istio & Linkerd CPU](https://medium.com/@michael_87395/benchmarking-istio-linkerd-cpu-c36287e32781#65ad)

## Further reading

- [What is Envoy](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)
- [Linkerd](https://linkerd.io/2/getting-started/)
