---
title: "IstioConf 21"
date: 2021-02-25T09:17:47+01:00
draft: true
comments: true
toc: false
images:
tags:
- cncf
- cloudnative
- kubernetes
- sre
- devops
categories:
- Conferences
---

ICYMI, [IstioCon '21][1] was happening previous week 22-26 Feb 2021. Luckily the videos are still there, so if you want to check what's
been discussed you can still do it, it's nice that you can login with  Apple ID also :wink:

# Talks I found interesting

* [I want to sketch a mesh for you][2] by Christian Posta
    * installing the Istio control plane with the revision flag `istioctl install ... --revision 1-0-3`
        * version istio control planes & components to separate them from each other, as an operator of a system,
          the service mesh is a critical component of that system
        * canary upgrades through both Control Planes where workloads are managed by each of them
        * 

# Wrap up

[1]: https://events.istio.io/istiocon-2021
[2]: https://www.crowdcast.io/e/istiocon-2021/3
