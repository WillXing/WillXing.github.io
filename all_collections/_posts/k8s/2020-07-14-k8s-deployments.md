---
layout: post
title:  "Kubernetes deployments"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-14T00:00:00Z
---

Deployment it's a configuration to pass user's requirement to [master]({% post_url k8s/2020-07-13-k8s-master %}).

Then k8s master will find [node]({% post_url k8s/2020-07-13-k8s-node %}) and deploy [pod]({% post_url k8s/2020-07-10-k8s-pods %}) with app on the node, also make sure it's keep running.


## Command
- Create deployment: `kubectl create deployment [deployment name] --image=[image]`
- Change the image of existing deployment: `kubectl set image deployments/deployment-name deployment-name=image-name:verstion`
- Check the update rollout status: `kubectl rollout status deployments/deployment-name`
- Undo rollout: `kubectl rollout undo deployments/deployment-name`
