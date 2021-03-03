---
title: "Security Tools"
date: 2021-02-25T21:41:24+01:00
draft: false
comments: true
toc: false
tags:
- infosec
- security
- devsecops
categories:
- Tooling
---

I never had the chance to compact a list of available Security tools in Q1 2021, here it goes.
If something's missing from this list, anyone can freely open a PR and I'll gladly update the list.
# Security tooling available

By attending the ZapCon '21, I've received a feedback request email from them where
I should list security tools I've used in the past.

| Name | Pricing model |
| ---- | ------------- |
| [checkmarx.com][checkmarx] | Premium |
| [synopsys.com][synopsys] | Premium |
| [portswigger.net/burp][burp] | Premium/Community Edition |
| [detectify.com][dtfy] | Premium |
| [acunetix.com][acunetx] | Premium |
| [veracode.com][veracode] | Premium |
| [dependabot.com][dpnb] | Open-Source |
| [synk.io][synk] | Free/Standard/Pro/Enterprise |
| [stackhawk.com][swk] | Free/Pro/Enterprise |
| [hcltechsw.com/wps/portal/products/appscan][appscan] | Premium |
| [dependencytrack.org][dptk] | Open-Source |

## Opinions

Of all these tools I've heard good remarks on [dependabot][dpnb] mainly because it's Open-Source
since GitHub acquired it, plus it's fairly simple to integrate it into your CI/CD pipelines.

[dependencytrack][dptk] I saw it demoed in a real use-case scenario and what can you do with this tool
is absolutely amazing, you can configure the server to push dependency deprecations in your PRs (Pull Requests),
you have a pretty neat unified dashboard with any security vulns for your apps, definintely recommend it; plus, it's
easy to set-up as they offer a Docker image and it's licensed under Apache 2.0.

[veracode][veracode] is amazing for scanning your OnPrem and this is just one use-case, there are plenty more out there,
main point is that you should know how/what to configure properly so that you get a good experience from it (like all
tools nonetheless)

## What you get

Depending on your needs, don't go for something Premium just because you're buying *the support*, trust me, that in
2021 is so overrated and Support is not how it used to be before, now when you open a ticket, if you're lucky :point_up:, you get
someone experienced handling your case, otherwise you'll get someone who constantly asks you for logs et. al. and then
just keeps escalating your issue to heavenly gates and you lose time & neurons...

Choosing something that's Open-Source has way more benefits because for most of the issues you could find a GitHub issue
raised by someone else, or find something on StackOverflow, ddg.co or whatnot... it's also good to share your experiences to the
community :wink:


[checkmarx]: https://www.checkmarx.com
[synopsys]: https://www.synopsys.com
[burp]: https://portswigger.net/burp
[dtfy]: https://detectify.com
[acunetx]: https://www.acunetix.com
[veracode]: https://www.veracode.com
[dpnb]: https://dependabot.com
[synk]: https://snyk.io
[swk]: https://www.stackhawk.com
[appscan]: https://www.hcltechsw.com/wps/portal/products/appscan
[dptk]: https://dependencytrack.org
