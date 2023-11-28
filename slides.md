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


# What is it?

<v-click>

- IF you have decided to create a [platform team](https://www.thoughtworks.com/radar/techniques/platform-engineering-product-teams)
  

</v-click>

<v-click>

- consider some of these tools as either examples or tools to help solve the provisioning problem

</v-click>



<!--
CI and CD are important as a means to deliver incremental changes to improve learning and feedback and deliver value.
These pipelines often use elevated privileges (instead of people) in order to do automated steps.
They suggest using OpenID Connect instead of long lived high privilege token.

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
Last in the Radar as Trial
-->
---
layout: center
class: text-center
---

# Why do we care?

<div v-click class="text-xl">

Security
 
</div>
 

<!--
Security is everyone’s concern. If the accounts used for high level privileged actions are compromised they are not as easy to replace. It also means that the code that makes up the pipeline is now a liability as it can be changed with anyone with access to the repo… but may do extra or destructive actions.

-->
---
class: text-center
---

# Are there competing ways to solve the problem?

<div v-click>

<img
      class="absolute top-50 left-85 right-0 bottom-0"
      src="Jenkins.png"
      alt=""
    />
 
</div>
 

<!--
You can always segment off your build pipeline to a dedicated build process maintained by another team so that those who use the build are not the ones maintaining the build capability. This can create another handoff and could be a bottleneck to changes.

-->
--- 
layout: center
class: text-center
---

# Risks, pitfalls, downsides, and anti-patterns?

 Permissions 'dungeon crawl' to get setup
 

<!--
In the security/usability graph it is much easier to give the ‘trusted” build system a high credential account in order to do whatever it needs to do for build, provisioning, deployment, etc. There could be a permissions dungeon crawl where each new step comes up to a wall where you have to figure out which permission to create or add in order to complete all the steps.

Validating for least privilege access at every step and with each change has its own overhead.
You may have to create additional cloud resources in order to set up the capability to get temporary credentials.
-->
--- 
layout: center
class: text-center
---

# My Ranking

<div v-click class="text-xl">

Trial (or even Adopt)
 
</div>
 

<!--
least privilege revocable access is a great goal to work towards. It may be an issue of what resources can use the access as provided by your cloud capability provider.

-->
---
---
# Learn More
[Tech Radar for this entry](https://www.thoughtworks.com/radar/techniques/summary/platform-orchestration) ·
[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev)
