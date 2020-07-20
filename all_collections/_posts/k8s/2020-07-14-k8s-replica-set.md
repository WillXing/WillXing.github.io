---
layout: post
title:  "Kubernetes replica set"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-14T00:00:00Z
---

I think the replica set is like a service running in kubernetes, which created by [k8s_deployments]({% post_url k8s/2020-07-14-k8s-deployments %}).

It will tell the cluster how many [pods]({% post_url k8s/2020-07-10-k8s-pods %}) will need to spin up, and it will spin enough pods to meet the desired capacity.

## Command

- List all replica sets: `kubectl get rs`
