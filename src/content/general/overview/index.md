---
linkTitle: Overview
title: Running Kubernetes on Giant Swarm
description: Here you learn how we set up things for you and what we manage, so you don't have to.
weight: 10
menu:
  main:
    identifier: general-overview
    parent: general
last_review_date: 2020-06-17
user_questions:
  - What's included with Kubernetes run by Giant Swarm?
  - What's not included with Kubernetes run by Giant Swarm?
aliases:
  - /basics/kubernetes-on-giant-swarm/
owner:
  - https://github.com/orgs/giantswarm/teams/team-phoenix
---

With Giant Swarm you get fully-managed Kubernetes clusters, which you can then use to deploy your containers as you see fit. You have full admin rights to your clusters through their API, so you can change anything that is accessible through the Kubernetes API. Changes that require configuration of the Kubernetes components themselves (e.g. starting the API server or kubelets with specific arguments) need to be set by the Giant Swarm Ops team. If you have specific needs or feedback, don't hesitate to [get in touch](mailto:support@giantswarm.io).

## What is included

Your clusters comes out-of-the-box as follows:

- High-availability Kubernetes control plane on AWS with clustered etcd (optional), single node control plane with resilient etcd
- Resiliently deployed worker nodes
- Full end-to-end encryption between Kubernetes components
- Regularly rolling keys for above-mentioned encryption
- Calico networking plugin installed (supports Network Policy API)
- Native CNI plug-ins on AWS and Azure
- CoreDNS installed
- All resources and feature gates (incl. alpha) enabled
- NGINX Ingress Controller - running inside your cluster (optional app)
- Monitoring
- Storage

## What is not included

There are some things not included in the cluster as managed by us:

- Additional user-space services like dashboards, logging, container registry, etc. are not installed (getting them running is [really easy]({{< relref "/app-platform/overview" >}}), though).

## High availability and resilience

As of workload cluster release v{{% first_aws_ha_controlplane_version %}} for AWS, [multiple control plane nodes]({{< relref "/advanced/high-availability/control-plane" >}}) with one
etcd cluster member each are active by default on AWS.

On other providers and in older workload cluster release for AWS, your clusters have a single running control plane node. However, the clusters are set up in a way that they keep running even if the control plane node is unavailable for a while (e.g. due to planned upgrades, failure, etc.). The only slight degradation you might notice is that while the control plane node is down, you cannot change the state of your pods and other resources. As soon as the control plane node is up again, you regain full control.

Furthermore, Kubernetes takes care of syncing your cluster with your desired state, so even when a node goes down (e.g. due to planned upgrades, failure, etc.), pods will get restarted/rescheduled if they are not running once it comes up again. If the node was away due to a network partition, the pods might still be running. In that case the scheduler might have added more pods to other nodes while the node was away so it will remove some pods to be consistent with your desired number of replicas.

## Specifics of your cluster

As part of managing your clusters, we need to run some agents (e.g. for monitoring or storage) on them. Due to this fact some host ports might be already in use. Currently, this is limited to ports `10300` and `10301`, which are used by monitoring agents. If you run into issues, please [get in touch](mailto:support@giantswarm.io) and we will find a solution.

Similarly, some parts of the DNS, Ingress Controller, and Calico setups are visible to you inside your cluster. To ensure optimal running clusters, please refrain from manipulating the `kube-system` namespace as well as the pods and other resources running in them if they are not documented.

We customize the audit policy file to eliminate rules which are both low-risk and produce a high volume of log entries. For full details, check the manifest `audit-policy.yaml` in the repository [giantswarm/k8scloudconfig](https://github.com/giantswarm/k8scloudconfig) under `v_X_Y_Z/files/policies`.

### Specifics on AWS

On AWS all resources (besides minor resources on S3 and KMS) pertaining to a cluster carry the two following tags:

- `kubernetes.io/cluster/<clusterid>: owned`
- `giantswarm.io/cluster: <clusterid>`

Here, `<clusterid>` stands for the unique identifier of the cluster. This enables you to set up reporting to monitor usage and cost.
