# Istio

This Git repository is created with an intention to help developers learn Istio. Besides helping you with [Istio installation](Installation.md), it also summarizes the [underlying building blocks](PreReqs.MD), which once understood, will help you visualize how does Istio works, and what does it use internally to serve the purpose.

## Target Audience

The repository is prepared, primarily keeping below actors in mind.

- **Developers**, who intends to keep their code clean from operational concern
- **DevOps**, who wants control over release and traffic
- **Dev Managers**, who can advocate right practices

Besides, *not-so-technical* users can also leverage on conceptual level.

## Motivation

We've an application, composed to 4 different micro-services, using different technologies, and may also be developed/maintained by different teams. Blue lines indicate network communication between services.

![Without Istio](./resources/noistio.svg)

### Without Istio

Say, we're live in production with **reviews-v1** and team has **reviews-v2** ready for launch.

Now, we've couple of options to deploy **reviews-v2**. They're

1. Rollout v2 full scale, and prey that things work.
2. Do controlled incremental deployment, and never loose face or incur financial lose.

Obviously option 1 is a roller coaster. And certainly has below disadvantages

- Since **all** traffic now comes to reviews-v2, it may turn hard to monitor the performance and behavior of new components.
- In case something goes wrong, we spend nights in office.

**Managing above concerns without Istio**, or similar tool, ends with

- Functional services doing network-related things, like routing, failure handling etc.
- Additional code complexity
- Having different tech-stack adds to complexity of developing/maintaining code level network controls.
- Non-declarative way to manage network

### With Istio (or similar)

Istio works on the concept of side-car.

<img align="right" width="250" height="90" src="./resources/sidecar.jpg">

- Bike is your functional service, focused merely on business needs.
- Sidecar is Istio-proxy, managing all network concerns.
- `VirtualService` subscidies native Kubernetes `Service` and augments it's feature set.
- They reside is same pod, and thus lives & die together.

We can visualize this arrangement in below diagram, with grey boxes being Istio-proxies. Blue lines (i.e. network communication) is now happening via Istio-proxy containers.

![With Istio](./resources/withistio.svg)

Istio takes care of all non-business concerns - traffic management, security, policies, and observability. That means it's not code but Istio that manages what service version to call, and for whom, and with what ratio, and what to do if something doesn't work in between. Happy life!

- We inject Istio within our pods.
- Istio deploys proxy-containers along with functional services, and
- Istio-proxies take care of our all network related needs.
- And we'll live happy.
  - Our services only focus on business needs.
  - One solution for all side concerns, that works for all major technologies - that too totally abstracted away from functional services. 
  - DevOps remains always in control with declarative traffic management.
  - Happy customers, because we're in control and we don't break unexpectedly.
  - And with strategic deployment, we don't run into do-or-die situations.

## Learning path

### [Prerequisites](pre-req/Prerequisites.md)

Page briefs about the underlying terminology, and establishes certain facts.

### [Preparing the playground](pre-req/DockerDesktop.md)

We use **Docker Desktop** to run our demos.
However these demos can be performed with Minikube, and MiniShiift with very minimum changes.

### [Install Istio](./setup/istio/Installation.md)

Step by step walk-through on Istio installation. We also deploy sample test application, and verifies that all our containers are running along with Envoy pods.

### [Install Istio-managed sample app](./setup/bookinfo/BookInfo.md)

Introduction to sample Book-Info application, which is composed of four separate microservices, we'll use to demonstrate various Istio features.

### [Strategic deployment using Istio](./controlled-deployment/traffic-management.md)

Istio’s traffic routing rules let you easily control the flow of traffic and API calls between services.

#### [Dark Launch](./controlled-deployment/dark-launch/Readme.md)

Launching a feature, with control over who all can have access to it.

#### [Blue-Green Deployment](./controlled-deployment/blue-green-deployment/Readme.md)

A switch to toggle your live production and stagging environments.

#### [Canary Deployment](./controlled-deployment/canary-deployment/Readme.md)

Launching to a small portion of users first in live production to minimize a potential bug impact.

## My reading references

- [Istio / Docs](https://istio.io/latest/docs/)
- [Pluralsight | Managing apps on Kubernetes with Istio](https://app.pluralsight.com/library/courses/istio-managing-apps-kubernetes/table-of-contents) > [Elton Stoneman](https://github.com/sixeyed)
