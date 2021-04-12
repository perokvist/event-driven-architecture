---
layout: default
---

# Orchestrated containers, meets PaaS and Serverless

Orchestrator like Kubernetes has been the go to infrastructure for handeling more complex workloads.
Utilizing compute more evenly in cluster(s), scaling per unit. But this comes with some operational complexety. Different flavors of hosted orchestrator(s) is in every major cloud providers offering.

We also see orchestrator apis added on top of serverless infrastructure, to enable a elastic serverless cluster(s).

This eases some aspects of running applications in clusters, but the deployment and managment of the orchestrator still remains.

This is where projects like [OAM (open application model)](oam.dev) comes in. Through a developer centric model, it abstracts the unerlaying orchestrator expessing capabilites (traits) and application in one model.

This makes it possible to have one application centric model on top of different services.

In the Kubernetes case this is realized through [KubeVela](https://kubevela.io/), a Kubernetes plugin that uses OAM for deployments and enables an application centric PaaS way of deploying to Kubernetes.
This lets the "PaaS" be flexible, since it could add and express underlaying capablities of the platform as traits.

To futher add to the mix, lets say that dapr is installed in the cluster. Dapr could then be exposed as traits in the OAM model. Making the PaaS "daperized".

Now imagine all of this as a cloud offering!

*(Dapr support in KubeVela is target for its 1.1 release.*


