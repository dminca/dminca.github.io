---
title: "IstioCon 21"
date: 2021-02-25T09:17:47+01:00
draft: false
comments: true
toc: true
Cover: 004.jpg
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

## [I want to sketch a mesh for you][2] - by Christian Posta
![I want to sketch a mesh for you](/002.png)
* :sparkles: installing the Istio control plane with the revision flag `istioctl install ... --revision 1-0-3`
  * version istio control planes & components to separate them from each other, as an operator of a system,
    the service mesh is a critical component of that system
  * canary upgrades through both Control Planes where workloads are managed by each of them
* was doing the [istio-workshop][3]
* using HashiCorp Vault as your CA
* cool  AirPods Max, really good sound quality :thumbsup:
## [Improving Security with Istio][4]
* Alex Soto was screaming a lot, was funny

## [Taming Istio configuration with Helm][5]
* Ryan Michela showed off here other sides of Helm:
  * sometimes you find broken or incomplete charts
  * CRDs problem is not solved on both Helm or Kubernetes sides and sometimes results in
    intermitent installation failures caused by race conditions in K8s
  * writing charts is tedious
  * **You don't need most of Helm to get the most from Helm**
* :sparkles: Helm Starters -- ok, this is something that I just found out :thinking:
  * it's a Helm chart that makes helm charts :open_mouth:
  * `helm create my-service --starter some/thing`
* used [bookinfo][bookinfo] in the demos

## [Deep dive into Istio Auth Policies][6]
![Deep dive into Istio Auth Policies](/003.png)
* :sparkles: Lawrence Gadban just showed me that Istio has OPA (Open Policy Agent) built-in;
  not the real OPA, but it mimics that functionality with the Auth Policies
* Istio mTLS == Envoy at its core
* [SPIFFE][spiffe] -- doesn't matter what the accronyms mean, keep in mind that it's there to
  remove the need for app-level authentication & complex network level ACL config
## [Istio Debugging: Finding and fixing issues in a multi-cluster service graph][7]
* Eitan & Scott emphasized the fact that Service Mesh provides telemetry data OOTB
* your Single Pane of Glass (SPOG) collects telemetry data accross multiple envs
* what caught my eye was their (solo.io) definition for Single Pane of Glass:
  * a layer which aggregates all of your telemetry data in a single place
  * adds context for cross-cluster and hybrid-env data
  * creates actionable, useful metrics to help prevent and or solve outages
* show that SPOG & manually inject faults
  * helps understand & grok what's going on in the system

## [Debugging Istio within the Department of Defense][8]
![Debugging Istio within the Department of Defense](/001.png)
* Nick Nellis & Adam Toy gave this presentation, most interesting part was Adam's, here's why
* [DevSecOps managed services][8], team name's Platform One
* plain and simple application flow diagram (top to bottom)
* did a clear demo showing off how he introduced faults
* really valuable that often he showed on the slides where we're at in the whole app flow
* check the Response Headers of the 404 request in the WebInspector browser
* :sparkles: covers 90% of the issues [troubleshooting Istio][analyze] -> `istioctl analyze`

# Wrap up

Most interesting talk for me was [Adam Toy's][8] as I can clearly see that he repeated it multiple times, so
he was sure of all the steps he was following there.

I could only follow 2 days in a row with the live sessions, then I lost interest as I couldn't follow some
presentations (they were really bad prepared), so I decided to just watch the recordings.

Overall, this IstioCon '21 was a win, as I managed to learn quite some new stuff about Istio overall, well, tbh, most
important part for me was the troubleshooting and solving real life problems.

PS: everything marked with :sparkles: are stuff that I won after this Con, thanks guys for the great conference!

[1]: https://events.istio.io/istiocon-2021
[2]: https://www.crowdcast.io/e/istiocon-2021/3
[3]: https://github.com/layer5io/istio-service-mesh-workshop
[4]: https://www.crowdcast.io/e/istiocon-2021/5
[5]: https://www.crowdcast.io/e/istiocon-2021/21
[6]: https://www.crowdcast.io/e/istiocon-2021/24
[7]: https://www.crowdcast.io/e/istiocon-2021/35
[8]: https://software.af.mil/team/platformone/
[bookinfo]: https://istio.io/latest/docs/examples/bookinfo/
[spiffe]: https://spiffe.io
[analyze]: https://istio.io/latest/docs/ops/diagnostic-tools/istioctl-analyze/
