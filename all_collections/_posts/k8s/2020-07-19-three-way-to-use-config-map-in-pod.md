---
layout: post
title:  "3 ways to use configMap in Pod"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-19T00:00:00Z
---

## Load specific key from configMap

Use `valueFrom` and add `configMapKeyRef` under it, in the spec of the [pods]({% post_url k8s/2020-07-10-k8s-pods %}) container.

`configMapKeyRef` use 2 properties to find the value you need:
- `name`: for the configmap identifier/name
- `key`: value key in configmap

Here's an example:

```yaml
//...
env:
  - name: ready
    valueFrom:
      configMapKeyRef:
        name: statusConfig
        key: ready
```

## Load all config from a configMap

Use `envFrom`, and add `configMapRef` under it.

`configMapRef` need to set one field:
- `name`: configMap identifier

Here's an example:

```yaml
//...
envFrom:
  - configMapRef:
      name: statusConfig
```

## Load configMap to volume

In pod spec, use `volumes` and in the volume use `configMap.name` to create volume in pod.

Then in container, use `volumeMounts` to mount the volume created in pod.

```yaml
//pod ...
spec:
  volumes:
  - name: configvo
    configMap:
      name: statusConfig
  containers:
  - volumeMounts:
    - name: configvo
      mountPath: /path
```
