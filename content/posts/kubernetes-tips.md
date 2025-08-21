---
title: "Kubernetes poweruser tips"
date: 2022-08-12T17:08:36+02:00
draft: false
description: "Mostly command line hacks to speedup workflow"
tags: [ kubernetes, tips, til ]
---

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

# Delete a Pod stuck in "Terminating" state

{{< alert "triangle-exclamation" >}}
This is a dangerous operation. Be prepared in case you might corrupt the ETCD
database and have a backup. These resources might be in handy:
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#replacing-a-failed-etcd-member
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#securing-communication
{{< /alert >}}


If you did everything in order to delete a Pod, meaning delete its dependencies
and it's still not terminating, last resort is to attempt a removal from ETCD

## Identify the IPs of your ETCD replicas

Let's assume we have these IPs

```sh
10.11.68.181
10.11.68.226
10.11.68.164
```

and we want to terminate the Pod `oops-im-stuck-1234`

## Santity check: list the Pod

```sh
ETCDCTL_API=3 etcdctl --endpoints=https://10.11.68.181:2379,https://10.11.68.226:2379,https://10.11.68.164:2379 \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  get --prefix "/registry/pods/default/oops-im-stuck-1234"
```

## Delete the Pod from ETCD

```sh
ETCDCTL_API=3 etcdctl --endpoints=https://10.11.68.181:2379,https://10.11.68.226:2379,https://10.11.68.164:2379 \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  del "/registry/pods/default/oops-im-stuck-1234"
```

[1]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-undo-em-

