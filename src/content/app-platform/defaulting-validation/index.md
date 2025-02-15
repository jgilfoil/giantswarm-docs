---
linkTitle: Defaulting and validation
title: Defaulting and validation of App CRs
description: How defaulting and validation of app CRs is implemented by app-admission-controller
weight: 50
menu:
  main:
    parent: app-platform
last_review_date: 2022-02-16
owner:
  - https://github.com/orgs/giantswarm/teams/team-honeybadger
aliases:
  - /reference/app-defaulting-validation/
user_questions:
  - Is App CR defaulting and validation logic enabled for my cluster? 
  - How can I use App CR defaulting logic when installing Managed Apps?
  - What fields are validated in App CRs when installing or updating Managed Apps?
  - How can I ensure Flux is not blocked by the validating webhook?
---

## Overview

For Giant Swarm releases using app-operator version 3.0.0 and upwards the
defaulting and validation logic of [App CRs]({{< relref "/ui-api/management-api/crd/apps.application.giantswarm.io.md" >}}) is enabled.
This is the following releases:

- AWS >= v14.0.0
- Azure >= v13.1.0
- KVM >= v13.1.0

This logic is provided by
[app-admission-controller](https://github.com/giantswarm/app-admission-controller).
You can check if your cluster has this version of app-operator by checking the
release details in the web interface.

We have not enabled the defaulting and validation for existing App CRs to avoid
disrupting your current usage of the App Platform.

## Defaulting

The defaulting logic is implemented using a [mutating admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#mutatingadmissionwebhook). It currently
defaults the following settings in the App CR if they are not specified.

- The `app-operator.giantswarm.io/version` label - It determines which instance
of app-operator processes the App CR. The value is set based on the release
version of the cluster. e.g. `3.0.0`
- The cluster values configmap - It contains per cluster values like the base
domain for your cluster. e.g. `x7jwz-cluster-values`
- The kubeconfig secret - It lets app-operator connect to your cluster. e.g. `x7jwz-kubeconfig`

Here is an example App CR with only the settings you need to provide. The user
values configmap is optional, in case you wish to configure the app with your
own values.

```yaml
apiVersion: application.giantswarm.io/v1alpha1
kind: App
metadata:
  name: my-kong
  namespace: x7jwz
spec:
  catalog: giantswarm
  name: kong-app
  namespace: kong
  version: 0.7.2
  userConfig:
    configMap:
      name: kong-user-values
      namespace: x7jwz
```

Here is the App CR with the defaults added by the mutating webhook.

```yaml
apiVersion: application.giantswarm.io/v1alpha1
kind: App
metadata:
  name: my-kong
  namespace: x7jwz
  labels:
    app-operator.giantswarm.io/version: 3.0.0
spec:
  catalog: giantswarm
  name: kong-app
  namespace: kong
  version: 0.7.2
  config:
    configMap:
      name: x7jwz-cluster-values
      namespace: x7jwz
  kubeConfig:
    context:
      name: x7jwz
     inCluster: false
     secret:
      name: x7jwz-kubeconfig
      namespace: x7jwz
  userConfig:
    configMap:
      name: kong-user-values
      namespace: x7jwz
```

## Validation

The validation logic is implemented using a [validating admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#validatingadmissionwebhook).
It checks for common problems with App CRs.

Currently we validate:

- The `app-operator.giantswarm.io/version` label is present.
- All referenced configmaps and secrets exist.
- The catalog has a matching Catalog CR.
- There is no App CR with a conflicting annotation or label value for the [target namespace]({{< relref "/app-platform/namespace-configuration" >}}).

If app-operator finds a matching [AppCatalogEntry]({{< relref "/ui-api/management-api/crd/appcatalogentries.application.giantswarm.io.md" >}}) CR, it will use this to run more validation checks.

- Cloud provider compatibility (e.g. you can’t install the azure-ad-pod-identity app in AWS).
- Namespace restriction (cluster singleton, namespace singleton, fixed namespace).

## GitOps support

If you are managing your App CRs with Flux or a similar GitOps tool then the
validating webhook may block creation of the App CR if it is created before the
referenced configmap or secret.

To prevent this you can add the label `giantswarm.io/managed-by` and set the value
to the tool you're using e.g. `flux`.

```yaml
apiVersion: application.giantswarm.io/v1alpha1
kind: App
metadata:
  name: my-kong
  namespace: x7jwz
  labels:
    giantswarm.io/managed-by: flux
```

app-admission-controller will now allow the App CR to be created. app-operator
will still perform the validation checks and once the referenced configmap or
secret exists the app will be installed.

## Retry Logic

During cluster creation there can a short delay while the kubeconfig
secret and cluster values configmap are generated.

So if you have an automated process for creating App CRs please ensure
it retries and the App CRs will be created once these resources exist.
