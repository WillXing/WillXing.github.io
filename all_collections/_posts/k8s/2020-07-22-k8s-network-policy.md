---
layout: post
title:  "Kubernetes network policy"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-22T00:00:00Z
---

Network Policy is a resouce that kubernetes will use to control the access to the exposed services.

My understanding is this policy is actually control the traffic on the [pod]({% post_url k8s/2020-07-10-k8s-pods %}) level still, that's why need to set the `PodSelector`.

Network policy also can not created directly from the `kubectl`, need to use yaml/json template to create or apply by `kubectl`.

A simple example control the access to pod with label `run=nginx`, only allow resources with label `access=granted` to access is like following:

```yml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: access-nginx
spec:
  podSelector:
    matchLabels:
      run: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: granted
```
