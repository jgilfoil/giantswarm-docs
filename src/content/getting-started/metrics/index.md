---
linkTitle: Basic metrics
title: Metrics in your clusters
description: Recipe to enable a core metrics solution running on your Kubernetes cluster.
weight: 30
menu:
  main:
    parent: getting-started
aliases:
  - /guides/kubernetes-heapster/
user_questions:
  - How can I activate metrics-server in my clusters?
owner:
  - https://github.com/orgs/giantswarm/teams/team-teddyfriends
last_review_date: 2022-03-31
---

Getting core metrics like CPU and Memory usage of resources in your cluster is important not only for your own monitoring purposes, but also for extended functionality like horizontal Pod autoscaling.

The kubelet exposes core metrics of the nodes/pods via an endpoint, however, you need an additional monitoring tool to collect and expose those metrics. Until recently, the tool of choice for this was Heapster and its API. But with the recent move to a more general Metrics API you get such metrics directly from the Kubernetes API endpoint. However, for this to work you need to still run a monitoring tool that collects and exposes the metrics, as the Kubernetes API only aggregates monitoring backends to be exposed by it.

## Adding metrics with Metrics Server {#metrics-server}

[Metrics Server](https://github.com/kubernetes-sigs/metrics-server) is a cluster-wide aggregator of resource usage data, which it collects from the Summary API of the kubelets of each node. It registers itself in the main API server through Kubernetes Aggregator and thus is discoverable through the same API endpoint as the rest of Kubernetes under `/apis/metrics.k8s.io/`.

To deploy it, run:

```nohighlight
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

__Note__: For Metrics Server to work you need to have Kubernetes API Aggregator enabled on your cluster. This is enabled by default on clusters started after February 14th 2018. For older clusters you can use Heapster as described below.

## Use cases

There are some common cases where Core Metrics are used by Kubernetes:

- Horizontal Pod Autoscaler: it scales pods automatically based on CPU or custom metrics (not explained here). More information [here](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/).
- `kubectl top`: the command `top` of our beloved Kubernetes CLI display metrics directly in the terminal.
- Kubernetes dashboard: see Pod and Nodes metrics integrated into the main Kubernetes UI dashboard. 
- Scheduler: in the future, core metrics will be considered in order to schedule best-effort Pods.

## Further reading

- [Kubernetes Monitoring Architecture](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/monitoring_architecture.md)
- [Resource Metrics API](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/resource-metrics-api.md)
