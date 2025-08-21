---
title: "Things to Try"
date: 2025-08-15T11:53:19+02:00
draft: false
description: A checklist of things I would like to try out
summary: Collection of awesome tooling awesome to try out
tags:
- kubernetes
- tryout
---

1. [pepr.dev][1]
    - Mutate: Dynamically modify incoming resource configurations to adhere to system security policy
    - Validate: Enforce strict validation rules, preventing the deployment of resources that don't meet defined criteria
    - Watch: Enable rapid responses to updates and help maintain desired cluster states
2. [Flux Multitenant Policies][2]
    - how to use the Kubernetes built-in validating admission engine to enforce policies on multi-tenant clusters managed by Flux
3. [Startx LimitRange (sxlimits)][3]
    - [Simplifying Kubernetes Limits Range with sxlimits | Medium][4]
    - The sxlimits command line can interact with a kubernetes cluster and allow operations on the LimitRanges resources like resize or adjust to update the spec.hard values
4. [kubectl plugin to print Kubernetes resource conditions][5]
    - kubectl plugin to print Kubernetes object resource conditions in a more human-readable format
5. [open-source JSON Editor][6]
    - JSON Crack is a tool for visualizing JSON data in a structured, interactive graphs, making it easier to explore, format, and validate JSON. It offers features like converting JSON to other formats (CSV, YAML), generating JSON Schema, executing queries, and exporting visualizations as images. Designed for both readability and usability


[1]: https://pepr.dev
[2]: https://fluxcd.control-plane.io/guides/flux-policies/
[3]: https://sxlimits.readthedocs.io/en/stable/
[4]: https://startxfr.medium.com/simplifying-kubernetes-limits-range-with-sxlimits-604a96eaaf2c
[5]: https://github.com/ahmetb/kubectl-cond
[6]: https://github.com/AykutSarac/jsoncrack.com
