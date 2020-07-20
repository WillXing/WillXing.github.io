---
layout: post
title: "How to create pod only deploy on certain Node"
author: "Will Xing"
categories:
  - kubernetes
date: 2020-07-15T00:00:00Z
---

Set label on node.

Then use `NodeSelector` in the pod spec, inside `NodeSelector` can put some labels to tell only deploy on certain node.

```yaml
spec:
  nodeSelector:
    labelName: labelValue
```

A useful command to check all specs explaination:

`kubectl explain pods.spec`