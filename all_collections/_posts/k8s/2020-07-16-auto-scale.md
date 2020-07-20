---
layout: post
title:  "Kubernetes auto scaling"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-16T00:00:00Z
---

For auto scale, can use command: `kubectl autoscale`.

You can basically set following things:
- minimal [pods]({% post_url k8s/2020-07-10-k8s-pods %}): `--min=1`
- maximal pods: `--max=10`
- cpu utilization: `--cpu-percent=80`

Once you created auto scale, you can check them by `kubectl get hpa` where `hpa` stands for horizontal pod autoscaler.

It's like other resources, that you can delete and describe or whatever.
