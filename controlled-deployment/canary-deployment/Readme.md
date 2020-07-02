# Canary Deployment

## Motivation

Deploying to production can be risky. Despite all the mitigation strategies we put in place—QA specialists, automated test suites, monitoring and alerts, code reviews and static analysis—systems are getting more complex every day. This makes it more likely that a new feature can ripple into other areas of your app, causing unforeseen bugs and eroding the trust customers have in you.

## History of Canary Deployment

The term “**canary deployment**” comes from [an old coal mining technique](https://www.smithsonianmag.com/smart-news/story-real-canary-coal-mine-180961570/). These mines often contained carbon monoxide and other dangerous gases that could kill the miners. Canaries are more sensitive to airborne toxins than humans, so miners would use them as early detectors. The birds would often fall victim to the gas before it reached the miners. This approach helped ensure the miners’ safety—one bird dying or falling ill could save multiple human lives.

## Benefits

There are several cases when using canary deployment is beneficial or inevitable:

- Your app consists of separate microservices that are updated independently and performance/behavior consistency must be monitored closely in live production.
- Your platform or service serves hundreds of thousands of visitors daily (online gaming, online banking, eCommerce, social media, banking etc), so a faulty update can result in huge financial and goodwill losses.
- Your product depends on some legacy or a third-party system that cannot be replicated within the testing or staging environment, so testing new functionality is possible only on live production servers.

In these cases, canary deployment is the only way to check if everything is alright before performing a full-scale update of your systems. It is very cost-efficient, too, as you don’t have to use two production environments in parallel, like when performing Blue-Green deployment.

## Canary Deployment using Istio

### Prep

Follow the steps from [Blue-green deployment](./../blue-green-deployment.md)

### Canary with 10% traffic to reviews-v2

Deploy [canary rules with 90/10 split](./productpage-canary-90-10.yaml)

```bash
kubectl apply -f ./productpage-canary-90-10.yaml
```

> Browse to <http://bookinfo.local/productpage> & refresh, mostly reviews-v1 responses with some reviews-v2

### Canary rollout

Gradually shift traffic to reviews-v2 with desired v1/v2 split ratios.

### Canary with sticky session

Deploy [sticking new behavior once aligned](./productpage-canary-with-cookie.yaml)

```bash
kubectl apply -f ./productpage-canary-with-cookie.yaml
```

> Browse to <http://bookinfo.local/productpage> & refresh - once you hit v2 that puts a cookie in your browser, and you'll always get v2
