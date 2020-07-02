# Dark Lunch

## What is dark lunch

Launching a feature, but limiting access to it.

## Who does it and why

Companies like Google, Facebook, Amazon to ensure that

- New feature doesn't cripple the running application, or degrade the performance, or in any way turn away customers.
- You can use it to test the new features' adoption, monitor it's impact on target group and validate your assumptions.

## How do we do it

Feature toggle, in one form or another. The toggles let us decide who all to let access the new feature.

## DEMO

### Deploy Istio & Book-Info

It'll reset Istio and Book-Info to initial version. You may skip if haven't really played around.

```bash
kubectl apply -f ../setup
```

### Deploy reviews-v2

Deploy [reviews service - v2](./reviews-v2.yaml)

```bash
kubectl apply -f reviews-v2.yaml
```

Verify deployment

```bash
kubectl get pods -l app=reviews

kubectl describe svc reviews
```

> Browse to <http://localhost/productpage> and refresh, requests load-balanced between v1 and v2

### Swichting to dark launch

Deploy [test user routing rules](./reviews-v2-tester.yaml):

```yaml
...
spec:
  hosts:
  - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: tester
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```

```bash
kubectl apply -f reviews-v2-tester.yaml

kubectl describe vs reviews
```

> Browse to <http://localhost/productpage> - all users see v1 except `testuser` who sees v2.
> Istio supports routing traffic using headers, url, cookies etc., and also allows exact or regex match.

### Doing the QA things

Since we now have reviews-v2 accessible to `tester` team only, our test team can freely test different scenarios, without impacting the business customers.

#### Mimic network delay

Test team wants to verify how existing application will behave in case the newer reviews service version doesn't provide timely response. Will it hang the complete page, or just the subset of it.

Deploy [delay test rules](./reviews-v2-tester-delay.yaml).

```bash
kubectl apply -f reviews-v2-tester-delay.yaml
```

> Browse to <http://localhost/productpage> - `testuser` gets delayed response, all others OK

#### Mimic service fault

Well, it's obvious to assume that a new component may fail intermediantly. Let's test how my application will behave.

Deploy [503 error rules](./reviews-v2-tester-503.yaml)

```bash
kubectl apply -f reviews-v2-tester-503.yaml
```

> Browse to <http://localhost/productpage> -  `testuser` gets 50% failures, for all other users it works OK
