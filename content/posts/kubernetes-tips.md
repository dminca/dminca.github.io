---
title: "Kubernetes poweruser tips"
date: 2022-08-12T17:08:36+02:00
draft: false
description: "Mostly command line hacks to speedup workflow"
tags: [ kubernetes, tips, til ]
---

{{< badge >}}
Further tips will follow!
{{< /badge >}}

# Fetch revisions for a `StatefulSet`

Revisions for `StatefulSet` & `DaemonSet` are stored in `ControllerRevision`
objects.

```sh
kubectl get controllerrevisions --all-namespaces
```

These revisions allow [rolling back][1] `StatefulSet` by running

```sh
kubectl rollout undo
```

Different from `Deployment` which capture the state in `ReplicaSet`

kudos [Sheogorath](https://shivering-isles.com/til/2024/01/statefulset-revisions)

# Get events for a certain Pod

```sh
kubectl get events --field-selector involvedObject.name=<pod-name>
```

## Get events for other objects

```sh
kubectl get events --field-selector involvedObject.kind=<resource-name>,involvedObject.name=<object-name>
```

{{< alert "circle-info" >}}
* <mark>resource-name</mark> could be `Pod`, `Job`, `CronJob` etc.
* <mark>object-name</mark> the name of resource
{{< /alert >}}

# Delete a PVC stuck in "Terminating" state

You've probably already tried to forcefully delete the <abbr title="Persistent Volume Claim">PVC</abbr>

```sh
kubectl delete pvc <pvc-name> --force --grace-period=0
```

<abbr title="Persistent Volume Claim">PVC</abbr> needs to be unmounted from the
node in order to finalize deletion and we do that by annotating the object with
the removal of finalizer field.

```sh
kubectl patch pvc <pvc-name> -p '{"metadata":{"finalizers":null}}'
```

{{< button href="https://github.com/kubernetes/kubernetes/issues/69697" target="_blank" >}}Source{{< /button >}}

Also kudos [Dean Lewis](https://veducate.co.uk/kubernetes-pvc-terminating/)

[1]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-undo-em-

