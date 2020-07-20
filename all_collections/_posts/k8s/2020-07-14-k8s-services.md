---
layout: post
title:  "Kubernetes services"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-14T00:00:00Z
---

An abstract layer use to expose services from pods to cluster internal or external.

When use `ClusterIP` type to expose service, it's only available for cluster internal.

## Base on deployment

When exposing service, it's expose the [deployments]({% post_url k8s/2020-07-14-k8s-deployments %})

```sh
kubectl expose deployment/$DEPLOYMENT_NAME --type="ClusterIP/NodePort/ExternalName..."
```

## Use label

Service use [label]({% post_url k8s/2020-07-14-k8s-label %}) to select a group of [pods]({% post_url k8s/2020-07-10-k8s-pods %})
When expose a deployment, it will use deployment's automatically generated label.

## Load balancing

When multiple pods under exposed service, it will auto load balancing on those pods.

## Describe service

The expose port can find out by describe the serivce.

```sh
kubectl describe services/service-name
```

## Delete service

```sh
kubectl delete service -l labelkey=labelvalue
# eg. run=kubernetes-bootcamp
```
