---
layout: default
---

# Static web app on Azure

Using the edge/CDN for optimizing static assets is not anything new. Having web sites and web experinces consists of only static resource dates back to the dawn of the web.

Using tools for "pre-rednering" site with server like capabilities, and that "only" relay on api communication, is also not new, but cloud offerings catering to these kind of apps grow i numbers, with [Netlify](https://www.netlify.com/) being a key driver.

## JAM-stack
Apps build with javascript (behavior), api (for data build time and runtime) and markup (static), is often refered to as [JAM-stack](https://jamstack.org/).
At app level refering to or targeting a stack might be applicable but for scenario with composition, the tool for creating the resource becomes blured, and this becomes local patterns.

## Composition
Using the pillars of the web (html/css/js) as static or SSR resource also open the door for composition using transcusion of markup.

## Performance
One aspect of the static approch is of course that the resource is "ready", making it cheap to render for the browser and easy to cache or store in the edge.
The cost of behavior (javascript) is still there if used. Progressive enhancement is a good fit. For content there is also the option to re render the static resource on change of dependant data, instead on relying on client side fetch and templating.

### Build vs application
One challange is that many frameworks use/needs build steps to "pre-render" on changes, mixing application responsabilities and build time responsabilities.
Where you could compare this as a trigger for application code to a github web hook, triggering a build action.

### Event driven
Using SSR an option of rendering a part and push to the edge is an option, and of course something a framework or platform could support.

This behavior could also be compared to SSR with CDN in front where you purge parts of the CDN on changes.

> So it all comes down to push or pull strategies using an api and/or SSR optional static resources.

## Azure

Let focus on static sites and assets in Azure, ignoring the PaaS offerings for serving web apps.

- [Azure Static Web Apps](https://azure.microsoft.com/en-us/services/app-service/static/)
- [Static Web site in Azure Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)

### Azure Static Web Apps






### Sample

For a more technical example, this [sample](https://github.com/perokvist/Dapr.WebPush) shows how to run dapr and an app side by side in Azure Container Instances.

