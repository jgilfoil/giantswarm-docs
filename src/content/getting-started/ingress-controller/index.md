---
linkTitle: Installing an ingress controller
title: Installing an ingress controller
description: How to install the NGINX ingress controller using the Giant Swarm web user interface.
weight: 50
menu:
  main:
    parent: getting-started
user_questions:
- Which workload clusters ship with a pre-installed ingress controller?
- How do I install my own ingress controller?
- Which ingress controller is available via the Giant Swarm app platform?
aliases:
  - /guides/installing-optional-ingress-controller/
owner:
  - https://github.com/orgs/giantswarm/teams/team-cabbage
last_review_date: 2021-09-01
---

An ingress controller helps you expose your services to the outside world.

## Which workload cluster releases do not ship with an ingress controller

Clusters on our AWS, Azure, and KVM (On-premises) installations that have a workload cluster release version newer than `10.0.0` (AWS), `12.0.0` (Azure), and `12.2.0` (KVM) ship without an ingress controller by default. (Related: [Preinstalled and optional apps in workload clusters]({{< relref "/general/releases/index.md#apps" >}}))

That allows you full control to choose which and how many ingress controllers you
want to run on your cluster.

## How do I Install my own ingress controller

Using our Web UI you can install an NGINX ingress controller using our App Catalog.

1. Click "Install app" from the "Apps" tab when viewing your cluster
  ![Cluster detail screen showing install app button](cluster-detail.png)

2. Search for "nginx-ingress-controller-app" in the list of apps
  ![List of app catalogs including the Giant Swarm Catalog](app-list.png)

3. Select "nginx-ingress-controller-app" from the "Giant Swarm Catalog"
  ![List of apps in the Giant Swarm Catalog](app-search-result.png)

4. Click "Install in cluster"

5. Click "Install app" (In case you want any special configuration, this is where you can also provide a 'values.yaml' with your customized settings)
  ![App installation modal](install-app-modal.png)

After a few moments, the NGINX ingress controller should be running on your cluster.

More information about the nginx-ingress-controller-app can be found in the [nginx-ingress-controller-app](https://github.com/giantswarm/nginx-ingress-controller-app) repository.

## Further reading

- [Accessing pods and services from the outside]({{< relref "/getting-started/exposing-workloads/index.md" >}})
- [Running multiple NGINX ingress controllers]({{< relref "/advanced/ingress/multi-nginx-ic/index.md" >}})
- [Advanced ingress configuration]({{< relref "/advanced/ingress/configuration/index.md" >}})
