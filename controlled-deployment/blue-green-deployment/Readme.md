# Blue/Green Deployment

## Motivation

One of the challenges with automating deployment is the cut-over itself, taking software from the final stage of testing to live production. You usually need to do this quickly in order to minimize downtime.

## Approach

The blue-green deployment approach does this by ensuring you have two production environments, as identical as possible. At any time one of them, let's say `blue` for the example, is live. As you prepare a new release of your software you do your final stage of testing in the green environment. Once the software is working in the `green` environment, you **switch the router** so that all incoming requests go to the green environment - the blue one is now idle.

## Benefits

This technique can eliminate downtime due to app deployment. In addition, blue-green deployment reduces risk: if something unexpected happens with your new version on Green, you can immediately roll back to the last version by switching back to Blue.

## Going blue/green using Istio

### Prep

Deploy Istio & BookInfo:

```bash
kubectl apply -f ../setup/
```

### Deploy reviews-v2

Using existing gateway:

```bash
kubectl describe gateway bookinfo-gateway
```

Deploy [v2 product page with test domain](./productpage-v2.yaml):

```bash
kubectl apply -f ./productpage-v2.yaml
```

Check deployment:

```bash
kubectl get pods -l app=productpage

kubectl describe vs bookinfo

kubectl describe vs bookinfo-test
```

Add `bookinfo.local` domains to hosts file:

```bash
# On Windows
cat C:\Windows\System32\drivers\etc\hosts

# On Linux or Mac, add to `/etc/hosts`
sudo nano /etc/hosts

# Add followings to /etc/hosts file
127.0.0.1       bookinfo.local
127.0.0.1       test.bookinfo.local
```

> Browse to live v1 set at <http://bookinfo.local/productpage>

> Browse to test v2 site at <http://test.bookinfo.local/productpage>

### Blue/green deployment - flip

Deploy [test to live switch](./productpage-test-to-live.yaml)

```bash
kubectl apply -f productpage-test-to-live.yaml
```

Check live deployment:

```bash
kubectl describe vs bookinfo
```

> Prod/live is now v1 at <http://bookinfo.local/productpage>

Check test deployment:

```bash
kubectl describe vs bookinfo-test
```

> Test is now v1 at <http://test.bookinfo.local/productpage>

### Blue/green deployment - flip back

Deploy [live to test switch](./productpage-live-to-test.yaml)

```bash
kubectl apply -f productpage-live-to-test.yaml
```

## Further Reading

- [Blue green deployment - Martin Fowler](https://martinfowler.com/bliki/BlueGreenDeployment.html)
- [Istio: Request Routing](https://istio.io/latest/docs/tasks/traffic-management/request-routing/)
- [Istio: Traffic Shifting](https://istio.io/latest/docs/tasks/traffic-management/traffic-shifting/)
