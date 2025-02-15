---
# Generated by scripts/aggregate-changelogs. WARNING: Manual edits to this files will be overwritten.
aliases:
- /changes/tenant-cluster-releases-kvm/releases/kvm-v13.1.0/
changes_categories:
- Workload cluster releases for KVM
changes_entry:
  repository: giantswarm/releases
  url: https://github.com/giantswarm/releases/tree/master/kvm/archived/v13.1.0
  version: 13.1.0
  version_tag: v13.1.0
date: '2021-03-10T14:26:09+00:00'
description: Release notes for KVM workload cluster release v13.1.0, published on
  10 March 2021, 14:26
title: Workload cluster release v13.1.0 for KVM
---

This release upgrades QEMU to version 5.2.0 which results in scheduling improvements and better CPU limits enforcement.

## Change details


### kvm-operator [3.14.2](https://github.com/giantswarm/kvm-operator/releases/tag/v3.14.2)

#### Changed
- Use `k8s-kvm:0.4.1` with QEMU 5.2.0.



### app-operator [3.2.0](https://github.com/giantswarm/app-operator/releases/tag/v3.2.0)

#### Added
- Include `apiVersion`, `restrictions.compatibleProviders` in appcatalogentry CRs.
  
#### Changed
  
- Limit the number of AppCatalogEntry per app.
- Delete legacy finalizers on app CRs. 
- Reconciling appCatalog CRs only if pod is unique.
#### Fixed
- Updating status as cordoned if app CR has cordoned annotation.



### cluster-operator [0.24.2](https://github.com/giantswarm/cluster-operator/releases/tag/v0.24.2)

#### Changed
- Migrate to Go modules.
- Update `certs` package to v2.0.0.
- Refactor to use slightly newer dependency versions.



### cert-operator [1.0.1](https://github.com/giantswarm/cert-operator/releases/tag/v1.0.1)

#### Fixed
- Add `list` permission for `cluster.x-k8s.io`.



### chart-operator [2.9.0](https://github.com/giantswarm/chart-operator/releases/tag/v2.9.0)

#### Added
- Use diff key when logging differences between the current and desired release.
#### Fixed
- Stop updating Helm release if it has failed the previous 5 attempts.
