# Book-Info Application

This example deploys a sample application composed of four separate microservices used to demonstrate various Istio features.

Read more on BookInfo application here: <https://istio.io/latest/docs/examples/bookinfo/>

## Deploy BookInfo

Deploy the app ([bookinfo-v1.yaml](./bookinfo-v1.yaml)):

```bash
kubectl apply -f ./bookinfo-v1.yaml
```

And the gateway ([bookinfo-gateway.yaml](./bookinfo-gateway.yaml)):

```bash
kubectl apply -f ./bookinfo-gateway.yaml
```

## Verify deployment

Check pods:

```bash
kubectl get pods
```

Check gateway:

```bash
kubectl get svc istio-ingressgateway -n istio-system
```

> Browse to <http://localhost/productpage>

## Check pods have proxy auto-injected

Check the productpage proxy setup. You'll find that our pod is running with an additional `istio-proxy` container.

```bash
kubectl describe pods -l app=productpage
```

Find all proxy containers:

```bash
docker container ls --filter name=istio-proxy
```
