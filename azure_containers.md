---
layout: default
---

# Units of deployment - a brief look into the future guided by dapr

Application code has long been the deployment unit / unit of scale. Elevated in the cloud by PaaS offerings.
For the last decade containers has entered the scene as a better unit for platforms to work with.

## Containers

Containers as an development artifact has been around for a while. To ease development in some cases, being a key part in infrastructure and of course utilized with orcestrators.
On another hand, when you have an container infrastructure what kind of workloads should you run/manage? 
How do you leverage PaaS, FaaS, CaaS and SaaS offerings? 

Adopting containers to use as DX might seem straight forward, but could be a barrier in some environments, here we see projects like [Tye](https://github.com/dotnet/tye) pop-up, where the creation of the container(s) could be seen purely as a build artifact.

To use a container as the unit of scale could be seen as the defentition of CaaS, but we see utlization of containers be a part of many other cloud services, that is seen as PaaS or FaaS.

And while this is happening there is a new kid on the block, mainly driven by edge providers, where WASM is used as an deployment unit, in the same sense as a container, where a platform instead of docker as unit uses [WASM](https://webassembly.org/).

> *"If WASM+WASI existed in 2008, we wouldn't have needed to created Docker. That's how important it is. Webassembly on the server is the future of computing. A standardized system interface was the missing link. Let's hope WASI is up to the task!"* - [Solomon Hykes](https://twitter.com/solomonstre/status/1111004913222324225?s=20)


## Azure Services and Containers

In Azure we see more and more services adopting containers as an deployment option even into catered services. In some cases you could run the workload your self, but running it in a catered service reduces complexty and adds specialized features.

This could also be seen in the early announced integrations with Dapr. Even if the intial release might be targeting kubernetes, we see integrated services support containers, [docker compose](https://docs.docker.com/compose/) and in some cases even [HELM](https://helm.sh/).

- [Azure Logic apps integration](https://cloudblogs.microsoft.com/opensource/2020/05/26/announcing-cloud-native-workflows-dapr-logic-apps/)
- [Azure functions integration](https://cloudblogs.microsoft.com/opensource/2020/07/01/announcing-azure-functions-extension-for-dapr/)
- [Azure Api Gateway integration](https://cloudblogs.microsoft.com/opensource/2020/09/22/announcing-dapr-integration-azure-api-management-service-apim/)

You could also see [compose or helm support around app services](https://docs.microsoft.com/en-us/azure/app-service/quickstart-multi-container), and [Azure Container instances](https://docs.docker.com/engine/context/aci-integration/).

## Dapr
[Dapr](https://dapr.io/) could be run a process, but the main cases is a sidecar container (and possibly a set of containers for dapr services). So dapr could be placed side by side to nearly anything. This makes dapr's model attractive for integration into service offerings. It's "pluggable" component model allows cloud native development with more focus on the application than infrastructure.

>*"Dapr is a portable, event-driven runtime that makes it easy for developers to build resilient, microservice stateless and stateful applications that run on the cloud and edge and embraces the diversity of languages and developer frameworks."* - [dapr.io](https://dapr.io/)

Since it's targeting challanges within distributed system development, it's full potential today might be best fulfilled together with an orcestrator. But from a developers perspective that shouldn't matter. A perspective shared by [OAM](https://oam.dev/).

And together with OAM, one could imagine how future services could look like.

>*"An open standard for defining cloud native apps and building app-centric platforms."* - [oam.dev](https://oam.dev/)

Speaking of the future, imagine dapr running a WASM module, acting as a bridge between app and cloud, or in a low powered device that also could be managed by an orcestrator via [OCI](https://opencontainers.org/).

Read more about dapr and its future in [Learning Dapr](https://g.co/kgs/QaJk77) (book).

### Sample

For a more technical example, this [sample](https://github.com/perokvist/Dapr.WebPush) shows how to run dapr and an app side by side in Azure Container Instances.

