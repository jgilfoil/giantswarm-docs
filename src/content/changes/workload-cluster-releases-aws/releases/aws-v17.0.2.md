---
# Generated by scripts/aggregate-changelogs. WARNING: Manual edits to this files will be overwritten.
aliases:
- /changes/tenant-cluster-releases-aws/releases/aws-v17.0.2/
changes_categories:
- Workload cluster releases for AWS
changes_entry:
  repository: giantswarm/releases
  url: https://github.com/giantswarm/releases/tree/master/aws/v17.0.2
  version: 17.0.2
  version_tag: v17.0.2
date: '2022-03-10T11:21:44+00:00'
description: Release notes for AWS workload cluster release v17.0.2, published on
  10 March 2022, 11:21
title: Workload cluster release v17.0.2 for AWS
---

This release downgrades the version of the Flatcar AMI from `3033.2.2` to `3033.2.0` due to a bug in version `3033.2.1` -> `3033.2.3` preventing successful boot on some EC2 instance type families. (Notably the `m4` instance types)

* [Giant Swarm roadmap issue](https://github.com/giantswarm/roadmap/issues/891)
* [Upstream bug issue](https://github.com/flatcar-linux/Flatcar/issues/665)

**Note when upgrading from v16 to v17:** Existing `Vertical Pod Autoscaler` app installations need to be removed from the workload cluster prior to upgrading to v17 because the `Vertical Pod Autscaler` is provided as a default application. The two applications have different names which leads to them fighting each other.

## Change details

### containerlinux [3033.2.0](https://www.flatcar-linux.org/releases/#release-3033.2.0)
