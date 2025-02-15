---
# Generated by scripts/aggregate-changelogs. WARNING: Manual edits to this files will be overwritten.
aliases:
- /changes/tenant-cluster-releases-aws/releases/aws-v16.1.1/
changes_categories:
- Workload cluster releases for AWS
changes_entry:
  repository: giantswarm/releases
  url: https://github.com/giantswarm/releases/tree/master/aws/v16.1.1
  version: 16.1.1
  version_tag: v16.1.1
date: '2022-02-21T08:27:28+00:00'
description: Release notes for AWS workload cluster release v16.1.1, published on
  21 February 2022, 08:27
title: Workload cluster release v16.1.1 for AWS
---

This release provides a fix for `aws-ebs-csi-driver` to ensure all taints for `ebs-node` are tolerated as well as selecting the right node selector for all nodes.

## Change details


### aws-ebs-csi-driver [2.8.1](https://github.com/giantswarm/aws-ebs-csi-driver-app/releases/tag/v2.8.1)

#### Fixed
- Use node selector according to control-plane and nodepool labels.
