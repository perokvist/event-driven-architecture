---
layout: default
---

# Static web apps

Using the edge/CDN for optimizing static assets is not anything new. Having web sites and web experinces consists of only static resource dates back to the dawn of the web.

Using tools for "pre-rendering" sites with server like capabilities, that "only" relay on api communication, is also not new, but cloud offerings catering to these kind of apps grow i numbers, with services like [Netlify](https://www.netlify.com/) being a key driver.

In this article we'll navigate "static web" as a tool/pattern with composition and event-driven pattern and look at offerings for static web hosting by the major cloud providers. 

> This article focuses on static web sites created by a catered tool or SSR, not on SPA's nor "pre-rendering" in that context. Some services mentioned will support this case as well.

## JAM-stack
Apps build with javascript (behavior), api (for data build time and runtime) and markup (static), is often refered to as [JAM-stack](https://jamstack.org/).
At app level refering to or targeting a stack might be applicable but for scenarios with composition, the tool for creating the resource becomes blured, and this becomes a local pattern (if not using build time composition).

## Composition
Using the pillars of the web (html/css/js) as static or server-side-rendered(SSR) resource also opens the door for composition using transclusion of markup and assets.

## Performance
One aspect of the static approch is of course that the resource is "ready", making it cheap to render for the browser and easy to cache or store on the edge. 
The cost of behavior (javascript) is still there if used. Progressive enhancement is a good fit. For content there is also the option to re render the static resource on change of dependant data, instead on relying on client side fetch and templating.

- [JAMstack performance](https://css-tricks.com/a-look-at-jamstacks-speed-by-the-numbers/)

### Build vs application
One challange is that many platform/frameworks use/needs build steps to "re-render" on changes, mixing application responsabilities and build time responsabilities.
You could compare this to a trigger from application code to a github web hook, triggering a build action.

### Event driven
An event that triggers a build is one option.

Using SSR rendering a part and push to the edge is an option, and of course something a framework or platform could support.

This behavior could also be compared to SSR with CDN in front where you purge parts of the CDN on changes.

> Conclusion : It all comes down to push or pull strategies, utilizing the edge, leveraging apis, SSR and static resources ,for performance, SEO and composition. Combining patterns, and possible enable composition scenarios for both server-side-inclusion(SSI) and client-side-inclusion (CSI). In a simple application only one pattern might suffice, in a more complex scenario blending different patterns are applicable.

Lets look at the big three could providers and their support for static web apps. Staring with Azure and later compare the findings with GCP and AWS offerings.

## Static Web in Azure

Let focus on static sites and assets in Azure, ignoring the PaaS offerings for serving web apps.

- [Azure Static Web Apps](https://azure.microsoft.com/en-us/services/app-service/static/)
- [Static Web site in Azure Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)

### Azure Static Web Apps

This service is catered for static site generators combined with HTTP apis.

A github action run your generator and pushes the artifacts to the service, if you combine this with function in your project, they are deployed as well under the same domain.
With support for routes, login/security and different envionment this is a perfect match for static sites. Not to mention that is also globaly distributed by default.

But since the artifacts are not accessible trough any api/sdk, the only why to push changes is through the github action. 

### Static web site in azure storage

Azure storage has an option to create a "web" container where its assets will be served via a simple http server. Its then simple to add a CDN in front.
This makes this offering more "bare bone" with less features, but easly managed since you could change and push files via api/sdk.

## Static Web on GCP

Much like the azure storage option, where you push files to a bocket to serve, but on GCP you need a load balancer to add HTTPS support to your domain.
- [Hosting a static website on cloud storage](https://cloud.google.com/storage/docs/hosting-static-website)

## Static Web on AWS

- [Hosting static web on S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)
- [Hosting static web with AWS Amplify](https://aws.amazon.com/amplify/console/)

### Hosting static web with AWS Amplify
Like Azure Static Web Apps - A service catered to static web apps, with CI/CD support for popular frameworks. Domain, sercurity and globaly distributed. With integrations for build backend APIs.

### Hosting static web on S3

Like the CGP/Azure offering at this level. Host files from a S3 bucket, but with support for redirect, permission and logging. For HTTPS support you need cloud front.

> We cover services from major cloud providers here, with different feature sets, some with backend integration a long with broader integration of the providers offerings. In the space of hosting static web apps, there are several offerings from the CDN providers them selfs, such as [Cloudflare's worker sites](https://workers.cloudflare.com/sites).


