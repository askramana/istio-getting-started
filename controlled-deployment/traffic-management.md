# Traffic Management using Istio

## Summary

Istio’s traffic routing rules let you easily control the flow of traffic and API calls between services. Istio simplifies configuration of service-level properties like circuit breakers, timeouts, and retries, and makes it easy to set up important tasks like A/B testing, canary rollouts, and staged rollouts with percentage-based traffic splits. It also provides out-of-box failure recovery features that help make your application more robust against failures of dependent services or the network.

## Main Actors

Istio's traffic routing capabilities come from

- [Virtual services](https://istio.io/latest/docs/reference/config/networking/virtual-service/#VirtualService)
- [Destination rules](https://istio.io/latest/docs/concepts/traffic-management/#destination-rules)

You can think of **virtual services** as how you route your traffic to a given destination, and then you use **destination rules** to configure what happens to traffic for that destination. Destination rules are applied after virtual service routing rules are evaluated, so they apply to the traffic’s “real” destination.

**Virtual service** lets you configure how requests are routed to a service within an Istio service mesh, building on the basic connectivity and discovery provided by Istio and your platform (such as Kubernetes, or OpenShift). Further, a virtual service can consist of a set of routing rules that are evaluated in order, letting Istio match each given request to the virtual service to a specific real destination within the mesh.

## How does it help do controlled deployment

In a continuous deployment scenario, for a given service, there can be distinct subsets of instances running different variants of the application binary. These variants can be different service/API versions. Common scenarios where this occurs include *A/B testing*, canary rollouts, etc. 

The choice of a particular version can be decided based on various criterion (`headers`, `url`, `cookies` etc.) and/or by weights assigned to each version. Each service, we keep a default version consisting of all its instances.

## Demos

- [Dark launch](./dark-launch/Readme.md)
- [Blue/green deployment](./blue-green-deployment/Readme.md)
- [Canary deployment](./canary-deployment/Readme.md)

## Further reading

- [Traffic management](https://istio.io/latest/docs/concepts/traffic-management/)