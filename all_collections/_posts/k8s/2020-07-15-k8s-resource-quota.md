---
layout: post
title:  "Kubernetes resource quota"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-15T00:00:00Z
---

The resource quota is use to limit the resource in the `namespace`.

Once have the resource quota under the namespace, means the resource created need to obey the limitation in quota.

For create quota: `kubectl create quota quotaname --hard=cpu=1,memory=1G`.

## Multiple resource quota

In one namespace can have multiple quota, if 2 quota define limit of same resource, will take the smallest value. 