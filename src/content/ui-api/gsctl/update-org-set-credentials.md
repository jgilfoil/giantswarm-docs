---
linkTitle: update org set-credentials
title: "'gsctl update organization set-credentials' command reference"
description: The 'gsctl update organization set-credentials' command allows to set cloud provider credentials for an organization.
weight: 250
menu:
  main:
    parent: uiapi-gsctl
aliases:
   /reference/gsctl/update-org-set-credentials/
owner:
  - https://github.com/orgs/giantswarm/teams/team-rainbow
user_questions:
  - How can I set cloud provider credentials for an organization using gsctl?
last_review_date: 2021-01-01
---

{{% gsctl_deprecation_disclaimer %}}

Giant Swarm allows you to run clusters in your own cloud provider account/subscription. We refer to this as [multi-account functionality]({{< relref "/advanced/multi-account" >}}). As a prerequisite, the organization that should own the clusters has to be prepared with cloud provider credentials.

Please refer to our detailed guides on how to prepare roles and credentials in your AWS account or Azure subscription:

- [Prepare an AWS account to run Giant Swarm workload clusters]({{< relref "/getting-started/cloud-provider-accounts/aws" >}})
- [Prepare an Azure subscription to run Giant Swarm workload clusters]({{< relref "/getting-started/cloud-provider-accounts/azure" >}})

gsctl provides this command to store the credentials connected to your organization. After doing this, you will want to [create a cluster]({{< relref "/ui-api/gsctl/create-cluster" >}}) owned by the organization configured that way.

**Note:** The credentials of an organization are currently immutable. Once set, you cannot modify or remove them. However you can create and delete organizations at will.

## Usage

Regardless of your cloud provider, you need an organization to work with. If you haven't created one for the purpose yet, this can be done in our web user interface.

In this gsctl command, the flag `-o` or `--organization` specifies the organization to set the credentials for.

### For AWS {#aws}

For AWS you provide two ARN (Amazon Resource Name) strings for two separate roles. Here is the general syntax:

```nohighlight
gsctl update organization set-credentials \
  -o|--organization <org-id>  \
  --aws-operator-role <role-arn> \
  --aws-admin-role <role-arn>
```

The `--aws-operator-role` flag defines the role ARN for the role to be used by our software running the clusters in your account. The `--aws-admin-role` flag takes the ARN or a role to be assumed by our support staff members. Both are mandatory.

For example, if your organization name was `myorg` and your AWS account ID was `0123456789`, the command would be:

```nohighlight
gsctl update organization set-credentials \
  -o myorg  \
  --aws-operator-role arn:aws:iam::0123456789:role/GiantSwarmAWSOperator \
  --aws-admin-role arn:aws:iam::0123456789:role/GiantSwarmAdmin
```

### For Azure {#azure}

For Azure you provide four credential details. Here is the general syntax:

```nohighlight
gsctl update organization set-credentials \
  -o|--organization <org-id>  \
  --azure-subscription-id <subcription-id> \
  --azure-tenant-id <tenant-id> \
  --azure-client-id <client-id> \
  --azure-secret-key <secret-key> \
```

The flags mean:

- `--azure-subscription-id`: ID of your Azure subscription
- `--azure-tenant-id`: ID of the tenant to use within your subscription
- `--azure-client-id`: ID of the service principal to use by our software and staff
- `--azure-secret-key`: Secret key for the above service principal

## Related

- [Basics and Concepts: Multi-Account Support]({{< relref "/advanced/multi-account" >}})
- [Guides: Prepare an AWS account to run Giant Swarm workload clusters]({{< relref "/getting-started/cloud-provider-accounts/aws" >}})
- [Guides: Prepare an Azure subscription to run Giant Swarm workload clusters]({{< relref "/getting-started/cloud-provider-accounts/azure" >}})
- [API: Set credentials](/api/#operation/addCredentials)
