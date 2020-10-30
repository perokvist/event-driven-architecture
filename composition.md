---
layout: default
---

## Composition and Classification of micro frontends

Web composition gets a spotlight due to the  trending "micro frontends" term. There is a lot going under that umbrella term.
In this post we'll try to zoom-out and look at this larger puzzle and classify a lot off the different scenarios.

The first book in the subject is out - "Micro frontends in Action".

We also see the downside or miss use of micro front-ends pop-up in thought works tech radar in "micro frontend anarchy".

Nicolas Delfino has also been publishing a few new articles of the subject on his blog, expanding on the ideas, focusing on different aspects of a larger puzzle.

In this post we'll try to zoom-out and look at this larger puzzle and classify a lot off the different scenarios regarding web-composition, since the full scope is not something for a framework but much more about combining approaches, techniques and teams to solve technical and organizational problems, since ownership of data, templating and resources could be in different systems/teams.

## Pages
There need to be something on the other side of a user route.

A resource that could own content and/or behavior but utilizing composition. A page where we could compose.

Composition could be conditional on cookies/user/feature-set/etc.

- Layout - composition templates
- Cookies
- Login
- A/B test (conditional composition)

The page resource could be served static or processed (SSR).

Pages could also contain content and behavior that is not intended for sharing i.e., not taking part in composition else-ware.

## Content
Any resource with content (page or part/fragment).

Depending on the type of content different strategies for rendering and serving the resource is applicable.

### Content types
- Static
- Dynamic (cache)
- Dynamic - personal

### Templating options
Static
Client
SSR
### CDN Strategies
Pull
Push

### CDN/Edge
Utilizing a CDN adds options for caching, performance and composition*.

A CDN treats resources as a static cache, resources could be pushed to the cache/CDN or refreshed/pulled when expired.

This give you many different options in handling rendering content and handling changes of content.

With very dynamic/ad-hoc content the cache is insufficient.

The "readiness" of the content, how much processing is left to the client, is also something to considerate for over-all performance and efficiency of the cache.

In addition to the push/pull options we see static site generators for CMS and blogs using push on build options (with hydration, see next section).

There is probably not one approach to apply over-all in a more complex scenario, rather you want to mix and match.

## Behavior
Adding behavior to a page or fragment introduces new considerations.

Runtime dependencies that might break different parts and have negative impact on performance. Execution on client or server.

### Behavior types
- Over content
- Component
- Forms
### Styles
- Intrusive
- Progressive
- Hydration

### Hydration
Process popularized by "isomorphic" applications where rendering is made both on the server and on the client. Usually using Node on the server to maximize code reuse.

Due to the cost of delaying TTI by hydrating a complete tree, efforts are being made by front-end libraries to achieve partial / progressive rendering.

## Behavior - coupling - complex
In a more complex, full app experience there might be other considerations, where a "global" runtime and component model is more suited.

But even here a complete solution/system, might be a set of web sites and web apps.

### Indicators
- Component -> Component communication
- App like behavior

### SPA/Applications
This is an area where traditionally SPA was intended, hence single page applications. But the ever-changing eco-systems these days tend to be complex and costly.

Since a lot off apps are catered for internal use within companies/enterprises, where adoption of client/front-end competence is expensive to gain and to keep, we now see attraction to WASM based app models like Blazor.

Some SPAs could be seen as a shell, so there is a option for refactoring components towards micro frontends.