---
layout: default
---

# Azure Container Apps

Azure Container Apps (ACA) is one of the more interesting services in the Azure offerings. A platform service offering the benefits of open-source tech like Kubernetes, KEDA and dapr as a platform service. 

ACA PaaS is built on top of AKS (Azure Kubernetes Service), targeting hosting and operating applications that consist of one or more containerized service(s).

A PaaS built on these open source initiatives, makes a good starting point for "cloud native" applications.

## Platform
ACA, running a best practices K8 cluster, without access to the underlying apis, this could be a foundation for a small platform team to utilize or act as a platform for a smaller org without platform team(s). Even as a platform on top of kubernetes (AKS) there are still a few moving parts.

If your platform needs more control/customization than the service allows, moving to AKS or any other managed (CNCF) Kubenetes, should be viable. This is also an area where utilizing dapr comes into play. Since dapr is made for CNCF Kubenetes, this could also be installed on your target cluster, on other cloud providers or on premise.

A PaaS built on these open source initiatives, makes a good starting point for "cloud native" applications.

If in a "stream aligned" team it is preferable to keep a sensible amount of Kubernetes knowledge.

## Concepts 
When using ACA, you first need an ACA environment, an environment could be used to host multiple apps. Environment could also be configured to run on a pre-existing VNET. Environment is the equivalent to a Kubernetes namespace.

![env](https://learn.microsoft.com/en-us/azure/container-apps/media/environments/azure-container-apps-environments.png)

An app is most commonly one container deployed as a pod. It could be multiple containers (advanced)(as side cars or infrastructure). So an app is more in the scope of a service. When building systems/apps that consist of multiple services they are commonly deployed in the same environment, sharing vnet.
 
## Communication
Apps/service could communicate with each other in a few different ways. Ingress is per app/service where you could have public or private FQDN addresses letting service use either of those. Revisions and traffic management is also configured per app/service and specified or honored in apps to app communication.(*when using dapr the app name, [shared](https://github.com/microsoft/azure-container-apps/issues/372) between revisons, is used for service discovery and communication*)

## Deployment
Deployment from CI/CD pipelines commonly goes through a container registry, replacing the current revision or creating a new one (depending on revision mode).

In the case of deploying multiple apps/services at the same time, a revision-suffix could be used, but beware of the communication patterns if you have breaking changes in dependencies. 

Revision also allows for blue/green deployments.

## Ingress
Ingress controls access to the apps internal/public. Commonly one service might act as a gateway and be the only entry point for public traffic. If another service resides on the same vnet you could use gateway services. 

A custom gateway service could also use dapr integration for service discovery, resilience and other capabilities of darp.

- [Secure net microservices with azure container apps and dapr](https://medium.com/vx-company/secure-net-microservices-with-azure-container-apps-and-dapr-e122c6ea0aac)

## Scaling
Scaling is a major feature, since this introduces the serverless billing model of scaling to zero. Another is the utilization of KEDA, an autoscaler with a lot of scaling triggers as illustrated in these scenarios.

![scaling + scenarios](https://learn.microsoft.com/en-us/azure/container-apps/media/overview/azure-container-apps-example-scenarios.png)

## Observability 
**Application logging** (console output and system logs) could be streamed through the portal real time. The default storage is Azure Monitor Log analytics, where you later could query these logs. It's also possible to route these logs through Azure Monitor to other destinations.

Azure Monitor also collects metric data from your containers regularly.

Application logging via application insights could also be added by using SDKs.

Dapr logs are also collected into Azure monitor (*trough its internal open telemetry otel collector*)

Apps in the same environment are deployed in the same virtual network and write logs to the same Log Analytics workspace.

### Health
Coming from Kubernetes there is nothing new, since health probes in ACA are Kubernetes health probes. Where you could have one of each;

- Startup (perform additional startup tasks)
- Readiness (ready to take traffic)
- Liveness (is operational)

These probes support HTTP and TCP.

## Programming Model
With multiple applications, scaling models and building blocks triggered through dapr, ACA + Dapr could be seen as one programming model that covers multiple scenarios, where one might replace multiple services with different models, like functions and wep apps, with ACA + dapr (sdk). This could also aid in portability.

This view of the programming model seems like a good fit for stream aligned teams.

## Integration with other services

#### Azure Static Web Apps

ACA integrates as an option for backing apis for Azure Static Web Apps (ASWA).

ASWA being a purpose built CDN solution for web framework using static resources working with data apis - JAM-Stack.

ACA is one of the services with integrations with Azure Static Web Apps (ASWA), an option catered for serving data for other frameworks in ASWA. Since ASWA is built around static resources, being an option of caching and processing on the edge is not viable. 

A classic CDN might aid in caching for ACA apps, but it will be interesting how these services or some other service will approach edge processing.

#### Authentication
Authentication and Authorization aka "Easy Auth" is also integrated with ACA. When enabling that it runs as a side car for each application.

![easy auth as side car](https://learn.microsoft.com/en-us/azure/container-apps/media/authentication/architecture.png)

#### Gateways

The [network](https://learn.microsoft.com/en-us/azure/container-apps/networking) options for ACA leaves some questions, mentioned [here](https://github.com/microsoft/azure-container-apps/issues/414). How do ACA integrate with services like, Application Gateway, Api Management or front door. ACA uses envoy ingress, you could always deploy a “custom” gateway as an app within your environment.

### Closing

ACA, a single platform service that caters for hosting web applications, background services, triggered functions and other processes in different stacks, but with an optional unified programming model through dapr, is a really interesting offering, challenging a lot of customer kubernetes setups out there.

There is also a momentum in regards to dapr, through start-ups like [Diagrid}(https://www.diagrid.io/).



