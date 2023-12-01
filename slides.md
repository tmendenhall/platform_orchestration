---
theme: default
class: text-center
layout: center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: fade-out
title: Tech radar 2023
mdc: true
---

# Platform Orchestration

Tech Radar 2023

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->
---
class: text-center
---


# What is it?
Platform Orchestration

<v-clicks>

- IF you have decided to create a [platform team](https://www.thoughtworks.com/radar/techniques/platform-engineering-product-teams)
- consider some of these tools for implementation or as examples of solutions to the provisioning problem
  

</v-clicks>
<!--
Platform engineering is the process of creating an environment for developers to create, test, and deploy capabilities into production.
Or to help manage the extrinsic cognitive load on the development teams.
Kubernetes is a “Platform for building platforms” as it has many useful primitives and abstractions.  These primitives are quite granular and need to be combined together in order to fulfill a given capability.
Platform teams usually work on “best practice” solutions to common problems which may involve homegrown tools to automate the creation of various capabilities. (when they are not provided by a separate service). 
Now we are seeing second generation tools that allow for the creation of self-service automated provisioning and configuration of resources at the developer level.
Two tools were suggested.
-->
---
layout: center
class: text-center
---

# Where did Thoughtworks rank it?

<div v-click class="text-xl">

Assess
 
</div>
 

<!--
New to the radar following the older blip of Platform Engineering.
-->
---
layout: center
class: text-center
---

# Why do we care?

<div v-click class="text-xl">

Developer Experience!
 
</div>
 

<!--
Capabilities have to live somewhere. Clients that are willing to invest in an environment for learning, capability deployment and operation could benefit from additional tooling to keep them from reinventing the wheel for capabilities. Building this environment as a company is expensive… and these tools can be expensive… but if you put them in the context of the full lifecycle of creation, modification, and deprecation then they start to make more sense.

-->
---
class: text-center
---

# Are there competing ways to solve the problem?

<div v-click>

<img
      class="absolute top-50 left-85 right-0 bottom-0"
      src="/jenkins.png"
      alt=""
    />
 
</div>
<div v-click>
  <h1>Just kidding</h1>
</div>

<!--


-->
---
class: text-center
---

# Thoughtworks Tool Recommendations

## [Humanitec Platform Orchestrator](https://humanitec.com/products/platform-orchestrator)

<v-clicks>

Hosted Sass for doing Any cloud resource configuration

Their abstraction is based on the "Score" [specification](https://github.com/score-spec/spec)

which can be translated into a compose file, a helm chart or anything else you define as a translation

</v-clicks>

<!--
Show their extremely slick web site.
-->
---
class: text-center
---
# Thoughtworks Tool Recommendations

## [Kratix](https://kratix.io)

<v-clicks>

Kubernetes specific tool targeted for Platform Engineers

"Kratix is the framework to deliver Platform-as-a-Product"

Their abstraction is a "Promise" which is the defined contract btween the platforms team and any capability development team to provide a resource.

"Any capability (that can be configured through Kratix) can be encoded and delivered via a promise" ..." and then is available on-demand at scale across the organization." -- from Colin Humphreys PlatformCon video 2022

Database, identity service, supply chain, composite pipeline of tools

</v-clicks>

<!--


-->
---
class: text-center
---
# Kratex
Definitions 

<v-clicks>

Promise (the interaction between the app team and the platforms team)

Custom resource definition. What you need from the app team to create a resource

Worker cluster resources: what you have determined is necessary to create the "Workload"

Request pipeline: business logic (optional) required when a resource is requested

All these operations run as operators in a kubernetes cluster

</v-clicks>

<!--
You determine the resource definition... like just the name, or more details that you see as necessary.

All other resources... their own dependenceis, databases, networking

A pipeline is a set of containers that you can run that does any other operations. Billing, scanning, compliance tracking.. etc.

-->
---
class: text-center
---

# Kratix

<img class="absolute top-10 left-12 right-0 bottom-0" src="/clusters.jpg">


<!--
Is this a bad thing... well if you are already into using kubernetes as a flexible platform for application services it is reasonable that you would also provide it as an environment for running other internal tools.
-->
---
---
# Kratix Promise definition

```yaml {1-10|31-50} {lines: true, maxHeight: '420px'}
apiVersion: platform.kratix.io/v1alpha1
kind: Promise
metadata:
  name: jenkins
spec:
  api:
    apiVersion: apiextensions.k8s.io/v1
    kind: CustomResourceDefinition
    metadata:
      name: jenkins.marketplace.kratix.io
    spec:
      group: marketplace.kratix.io
      names:
        kind: jenkins
        plural: jenkins
        singular: jenkins
      scope: Namespaced
      versions:
      - name: v1alpha1
        schema:
          openAPIV3Schema:
            properties:
              spec:
                properties:
                  env:
                    default: dev
                    description: |
                      Configures and deploys this Jenkins with Environment specific configuration. Prod Jenkins comes with Backups pre-configured.
                    pattern: ^(dev|prod)$
                    type: string
                  plugins:
                    default: []
                    description: Plugins to install in the requested Jenkins
                    items:
                      description: Plugin defines a single Jenkins plugin.
                      properties:
                        downloadURL:
                          description: DownloadURL is the custom url from where plugin
                            has to be downloaded.
                          type: string
                        name:
                          description: Name is the name of Jenkins plugin
                          type: string
                        version:
                          description: Version is the version of Jenkins plugin
                          type: string
                      required:
                      - name
                      - version
                      type: object
                    type: array
                type: object
            type: object
        served: true
        storage: true
  destinationSelectors:
  - matchLabels:
      environment: dev
  workflows:
    promise:
      configure:
      - apiVersion: platform.kratix.io/v1alpha1
        kind: Pipeline
        metadata:
          name: promise-configure
          namespace: default
        spec:
          containers:
          - image: ghcr.io/syntasso/kratix-marketplace/jenkins-configure-pipeline:v0.1.0
            name: jenkins-promise-pipeline
    resource:
      configure:
      - apiVersion: platform.kratix.io/v1alpha1
        kind: Pipeline
        metadata:
          name: instance-configure
          namespace: default
        spec:
          containers:
          - image: ghcr.io/syntasso/kratix-marketplace/jenkins-configure-pipeline:v0.1.0
            name: jenkins-configure-pipeline
```
---
---
# Requesting a Kratek Promise

```yaml {1|2|3-7} {lines: true}
apiVersion: marketplace.kratix.io/v1alpha1
kind: jenkins
metadata:
  name: example
  namespace: default
spec:
  env: dev
```
---
class: text-center
---
# Risks, pitfalls, downsides, and anti-patterns?

<v-clicks>

- In for a penny in for a pound... how big is your development team footprint and budget?
- The tools do not solve the prime mover problem of how you configure the clusters themselves
- The tools do not define how you interact with creating resources for configuration?
- Next level abstraction for consumers of the platform... but still requires the team using the tools to have extensive knowledge of the abstraction layers down, k8s primitives, helm templates. etc.
- Could help with multi-cluster or multi-cloud IF that is desired
- If you are focusing on one Cloud compute provider: Are these abstractions useful or can you use their own cloud provisioning tools?
 
</v-clicks>
 

<!--
gitops pipeline, backstage, etc?
--> 
--- 
layout: center
class: text-center
---

# My Ranking

<v-clicks class="text-xl">

<ul>
<li> Trial (If you already have a platforms team...) </li>
</ul> 
</v-clicks>
 

<!--
The Platforms team already has to work on what it takes to upgrade tools, improve capabilities, refine the interface and research new capabilities hypothesized. If this can free up more time away from request operations then good.
-->
---
---
# Learn More
[Tech Radar for this entry](https://www.thoughtworks.com/radar/techniques/summary/platform-orchestration) ·
[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev)
