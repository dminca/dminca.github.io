---
title: "Part 1: Multi-tenant Thanos with Jsonnet"
date: 2025-08-29T21:33:15+02:00
draft: false
description: "Part 1 - Deploy a multi-tenant monitoring architecture with Thanos and jsonnet"
tags:
- kubernetes
- tutorial
- monitoring
---

This guide has multiple parts, youre reading now **Part 1**. Had to do this to
prevent the blog post from getting way too huge to manage and also not to tire
the poor reader. The rest of the parts will be added below as they come

It all started with the "what if's"

- what if I don't need Helm to generate a shit ton of manifests and pick my
    brain on understanding whatever the fuck the author of the Chart wanted
    to say on this `_helpers.tmpl`
- what if Kustomize is simply not enough for keeping my code DRY because I have
    to apply too much brain gymnastics to format my code according to
    Kustomize's requirements
- what if THERE'S MORE THAN THAT?

Well, there was actually more: [jsonnet][1]

If you check your basic tutorial on YouTube or any blog post on the net, nearly
everyone keeps saying _YAML is the language of **data**_, well this is what's
built upon. Generating **data**, be it YAML or JSON.

# Toolchain

Like all carpenters out there, you're gonna need some basic tooling to get this
story working
- [Thanos][2]
- [kluctl][3]
- [jsonnet-language-server][4]
- [jsonnet-bundler][5]
- [kube-thanos | jsonnet][6]
- [kube-prometheus | jsonnet][7]

## Setting up your IDE (NeoVIM)

**DISCLAIMER(!)** althought `jsonnet` language support is obnoxiously crap
at the current point in time, you're not going to get 100% flexibility in other
IDEs except NeoVIM. Just wanted to put this out there so that we smash all
dreams from the beginning and **be real**.

1. install [jsonnet][1] CLI
2. install [jsonnet-bundler][5]
3. install [jsonnet-language-server][4]
    - configure it in your NeoVIM CoC, you can see my [Nix Flake for reference][8]

### Generating the YAML manifests

By default, `jsonnet -J vendor -m manifests example.jsonnet` command will dump
JSON files in the `./manifests` dir.

The `-J vendor` flag we're passing instructs jsonnet _where to find the
dependencies_ we've pulled from the net.

Therefore, we're going to need [gotojsonyaml][9] to convert the JSON files to
Kubernetes YAML manifests, as seen [in this `build.sh` script][10] that everyone
shoves up our throats as the _default way to go_.

### Bash scripts

Personally, I despise using Bash scripts simply for this operation of converting
JSON to YAML because the above mentioned shell script is fairly limited. Sure,
one can argue _it's just a starting point or example to get started_ BUT
you'll notice the script craps-out midway while rendering through [kube-prometheus][7]
`jsonnet` files ... 

That's why I've written a small Go program helper to tackle this operation in a
much safer and faster way see [codeberg.org/dminca/tiny-programs/jsonnet-convert][11]

Shell scripts have been for a long time considered the _glue code_ of architecture
but what this glue code will never contain is **UNIT TESTS**. Let's admit it,
no one in their lives that've written shell scripts, have written a single
unit test for them because the only supported way (which came fairly late) was
[bats][12] and it's horrendous.

Programming this in Go has many advantages
1. everything is contained in that package
2. unit tests sit close to the main code
3. dependencies are tracked
4. dependencies are cryptographically checksumed so ++Security

Can shell scripts do that? NOPE.

## Project structure

Since we mentioned we're going to use [kluctl][3] to tackle the deployment of
all generated manifests, we need to structure it in a way that's not getting
too messy to get out of control, here's what I propose

```
 .
├──  kube-thanos-jsonnet
│   ├──  manifests
│   ├──  vendor
│   ├──  first-tenant-example.jsonnet
│   ├──  jsonnetfile.json
│   ├──  jsonnetfile.lock.json
│   ├──  kube-prometheus-example.jsonnet
│   └──  thanos-example.jsonnet
├──  vars
│   ├──  common.yml
│   └──  prd.yml
├── 󰊢 .gitignore
├──  .kluctl.yml
├──  deployment.yml
└── 󰂺 README.md
```

This is the _most basic kluctl_ project structure that you could come up with

```yml
# .kluctl.yml
discriminator: "app.kubernetes.io/instance={{ target.name }}"

targets:
  - name: prd
    context: production-kubernetes-cluster-fqdn
    args: {}
  - name: stg
    context: staging-kubernetes-cluster-fqdn
    args: {}
```

```yml
# deployment.yml
vars:
  - file: ./vars/common.yml
  - file: ./vars/{{ target.name }}.yml

deployments:
  - path: kube-thanos-base
    git:
      url: https://github.com/thanos-io/kube-thanos
      ref: 6fedb045db2aeb0a4a880f77bfdfb5d4580f51f9
      path: jsonnet
```

Use `vars/common.yml` and `vars/prd.yml` to define environment specific variables
for Kluctl to use.

The rest of the project can be structured depending on what's needed

[1]: https://jsonnet.org
[2]: https://thanos.io
[3]: https://kluctl.io
[4]: https://github.com/grafana/jsonnet-language-server
[5]: https://github.com/jsonnet-bundler/jsonnet-bundler
[6]: https://github.com/thanos-io/kube-thanos
[7]: https://github.com/prometheus-operator/kube-prometheus
[8]: https://github.com/dminca/nix-config/blob/82acaffc70b4d45bc07167ecc3ed8337825ea36e/hosts/MLGERHL6W4P2RXH/neovim.nix#L18-L23
[9]: https://github.com/brancz/gojsontoyaml
[10]: https://github.com/thanos-io/kube-thanos/blob/6fedb045db2aeb0a4a880f77bfdfb5d4580f51f9/build.sh#L1-L32
[11]: https://codeberg.org/dminca/tiny-programs/src/commit/da243bd5527224f08e60e2d8cc590026cdde8850/jsonnet-convert/main.go#L1-L148
[12]: https://github.com/bats-core/bats-core
