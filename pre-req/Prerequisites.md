# Things to know | Before you start with Istio

## Conclusion

The whole talk primarily estabilishes following two facts.

1. Istio uses Envoy as edge and service proxy
2. Istio extends Kubernetes API (using Custom Resources and Custom Controllers) to provide traffic routing features.

Let's deconstruct the summary now.

---

## Envoy

Envoy is an open source **L7 edge and service proxy**, designed for cloud-native applications.

The majority of operational problems that arise when moving to a distributed architecture are ultimately grounded in two areas: networking and observability.

Originally built at **Lyft**, Envoy is a high performance C++ distributed L7 proxy designed for single services and applications, as well as a communication bus and “universal data plane” designed for large microservice “service mesh” architectures. Built on the learnings of solutions such as *NGINX*, *HAProxy*, hardware load balancers, and cloud load balancers, Envoy runs alongside every application and abstracts the network by providing common features in a platform-agnostic manner. When all service traffic in an infrastructure flows via an Envoy mesh, it becomes easy to visualize problem areas via consistent observability, tune overall performance, and add substrate features in a single place.

### References

- [Envoy - Getting started](https://www.envoyproxy.io/docs/envoy/latest/start/start)

## Proxy - Our old favorite

Proxies are everywhere! A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes.

In the proxy, extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked, or doing boilerplate before/after actual object call.

For the client, usage of a proxy object is similar to using the real object, because both implement the same interface. They're like salt and spices to the food.

### References

- [Wikipedia | Proxy pattern](https://en.wikipedia.org/wiki/Proxy_pattern)

## Sidecar Proxy

Applications and services often require related functionality, such as monitoring, logging, configuration, and networking services. These peripheral tasks can be implemented as separate components or services.

We can deploy components of an application into a separate process or container to provide isolation and encapsulation. This pattern can also enable applications to be composed of heterogeneous components and technologies.

This pattern is named **Sidecar** because it resembles a sidecar attached to a motorcycle. In the pattern, the sidecar is attached to a parent application and provides supporting features for the application. The sidecar also shares the same lifecycle as the parent application, being created and retired alongside the parent.

### References

- [Microsoft | Sidecar pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/sidecar)
- [Istio | Traffic management / Sidecar](https://istio.io/docs/reference/config/networking/sidecar/)

## Edge Routers

A router that enforces the firewall policy for your cluster. This could be a gateway managed by a cloud provider or a physical piece of hardware.

**Ingress** exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

### References

- [Kubernetes | Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [Kubernetes | Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)

## L4 and L7 Load Balancing

**L4 load balancing** offers traffic management of transactions at the network protocol layer (TCP/UDP). L4 load balancing delivers traffic with limited network information with a load balancing algorithm (i.e. round-robin) and by calculating the best server based on fewest connections and fastest server response times.

**L7 load balancing** works at the highest level of the OSI model. L7 bases its routing decisions on various characteristics of the HTTP/HTTPS header, the content of the message, the URL type, and information in cookies.

### References

- [OSI Model](https://en.wikipedia.org/wiki/OSI_model)
- [L4-L7 Network Services](https://avinetworks.com/glossary/l4-l7-network-services/)
- [NGINX: Benefits of Layer 7 Load Balancing](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)

---

## Extending Kubernetes APIs

Custom resources are extensions of the Kubernetes API. It represents a customization of a particular Kubernetes installation. Custom resource combined with custom controller provides a true declarative API.

A declarative API allows you to declare or specify the desired state of your resource and tries to keep the current state of Kubernetes objects in sync with the desired state. The controller interprets the structured data (i.e. a custom resource) as a record of the user's desired state, and continually maintains this state.

### References

- [Custom Resources and Custom Controller](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
- [Extend the Kubernetes API with CustomResourceDefinitions](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/)

---