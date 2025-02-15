---
linkTitle: create cluster
title: "'gsctl create cluster' command reference"
description: Detailed documentation on how to create a new cluster using the 'create cluster' command in gsctl.
weight: 40
menu:
  main:
    parent: uiapi-gsctl
user_questions:
  - What options are available for creating clusters through gsctl?
  - What are the defaults for the 'gsctl create cluster' flags?
  - How does an example call of 'gsctl create cluster' look like?
aliases:
  - /reference/gsctl/create-cluster/
owner:
  - https://github.com/orgs/giantswarm/teams/team-rainbow
last_review_date: 2021-01-01
---

{{% gsctl_deprecation_disclaimer %}}

The command `gsctl create cluster` allows to create new Kubernetes clusters.

You can use the command with very simple syntax to create a cluster using default values. Note that several cluster specification details cannot be changed after creation.

In order to configure all details of the cluster according to your requirements, you'll create a [cluster definition]({{< relref "/ui-api/gsctl/cluster-definition" >}}) first and pass it to the command.

## Command line examples

The first and rather trivial example shows how to create a cluster for organization `myorg` and specifying the cluster name, while leaving all other settings to defaults:

```nohighlight
gsctl create cluster --owner myorg --name "Test cluster"
```

The second example shows how to create a cluster where most or even all configurable details are specified, using a definition file:

```nohighlight
gsctl create cluster --file prod_cluster_definition.yaml
```

Note that command line flags take precedence over values in the definition. This way you can, for example, define a default cluster name in the definition, but set a specific one via the `--name` flag when applying the same definition several times.

## Full argument reference {#arguments}

- `--file`, `-f`: Definition file path. See [cluster definition reference]({{< relref "/ui-api/gsctl/cluster-definition" >}}) for details. The value `-` means that the definition will be read from standard input.
- `--owner`, `-o`: Name of the owner organization. Overwrites a name given per definition file.
- `--name`: Name of the cluster. Overwrites name given in definition file.
- `--release`, `-r`: Specific the workload cluster release version to use. Defaults to the latest active version. See [list releases]({{< relref "/ui-api/gsctl/list-releases" >}}) for details on listing available workload cluster releases.
- `--create-default-nodepool`: Where node pools are supported (AWS since workload cluster release v{{% first_aws_nodepools_version %}} and Azure since workload cluster release v{{% first_azure_nodepools_version %}}), setting this to `false` allows to suppress the creation of a default node pool. A default node pools would otherwise be created automatically if no cluster definition is given specifying any node pools details, to get you started quickly.
- `--master-ha`: Where supported, this is `true` by default, which means that the cluster will have three control plane nodes. Available on AWS since workload cluster release v{{% first_aws_ha_controlplane_version %}}. Set this to `false` to have only one control plane node in the cluster (recommended only for test clusters).
- `--output`: By specifying this flag with value `json`, the output can be printed in JSON format. This is convenient for use in automation. See [JSON output](#json-output) for examples.

## Passing the cluster definition via standard input {#stdin}

The `--file` (short `-f`) flag accepts the special value `-` to read a definition from standard input. Depending on your shell you can make use of that in various ways.

For example, you can use a pipe like this:

```nohighlight
cat my-cluster.yaml | gsctl create cluster -f -
```

At least in bash, this syntax allows to include the entire definition in the command:

```bash
gsctl create cluster --file - <<EOF
owner: acme
name: Dev cluster
release: 7.1.1
EOF
```

## JSON output {#json-output}

Passing flag `--output` with value `json` to `gsctl create cluster` changes the printed output to be formatted as a JSON object.

**Example success output:**

```nohighlight
{
  "result": "created",
  "id": "f01r4"
}
```

When requesting cluster creation with workload cluster release v{{% first_aws_nodepools_version %}} on AWS, or {{% first_azure_nodepools_version %}} on Azure, it is possible to recieve `created-with-errors` as value for the `result` key. This indicates some problem with either node pool creation or label attachment.

**Example error output:**

```nohighlight
{
  "result": "error",
  "error": {
    "kind": "unknown",
    "annotation": "Unauthorized",
    "stack": [
      {
        "file": "/go/src/giantswarm/gsctl/commands/create/cluster/v5.go",
        "line": 140
      },
      {
        "file": "/go/src/giantswarm/gsctl/commands/create/cluster/command.go",
        "line": 632
      }
    ]
  }
}
```

## Related

- [Cluster definition reference]({{< relref "/ui-api/gsctl/cluster-definition" >}})
- [`gsctl list releases`]({{< relref "/ui-api/gsctl/list-releases" >}}) - List all available workload cluster releases
- [`gsctl create kubeconfig`]({{< relref "/ui-api/gsctl/create-kubeconfig" >}}) - Getting a key pair and enabling `kubectl` to access a cluster
- [`gsctl delete cluster`]({{< relref "/ui-api/gsctl/delete-cluster" >}}) - Deleting a cluster
- [Basics: Cluster Size and Autoscaling]({{< relref "/getting-started/cluster-size-autoscaling" >}})
- [API: Create cluster (v4)](/api/#operation/addCluster)
- [API: Create cluster (v5)](/api/#operation/addClusterV5)
