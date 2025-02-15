---
linkTitle: select endpoint
title: "'gsctl select endpoint' command reference"
description: The 'gsctl select endpoint' command selects an endpoint for usage in subsequent command executions.
weight: 190
menu:
  main:
    parent: uiapi-gsctl
aliases:
  - /reference/gsctl/select-endpoint/
owner:
  - https://github.com/orgs/giantswarm/teams/team-rainbow
user_questions:
  - How can I select an API endpoint for use with gsctl?
last_review_date: 2021-01-01
---

{{% gsctl_deprecation_disclaimer %}}

The `gsctl select endpoint` command selects a Giant Swarm REST API endpoint for
usage in subsequent command executions. This defines which endpoint you use,
unless an endpoint is specified on a per-command basis using the
`-e`/`--endpoint` flag.

This is relevant to you only if you use several installations, e. g. one
on-premises and one in the cloud.

The command works basically as a switch between several available endpoints
that are maintained in your local gsctl configuration file. An endpoint is
added to your configuration whenever you use the [`gsctl login`]({{< relref "/ui-api/gsctl/login" >}})
command with a new endpoint URL. [`gsctl select endpoint`]({{< relref "/ui-api/gsctl/select-endpoint" >}})
helps you switch between several endpoints while staying logged in.
[`gsctl list endpoints`]({{< relref "/ui-api/gsctl/list-endpoints" >}}) and [`gsctl info`]({{< relref "/ui-api/gsctl/info" >}})/) give
you more information on status and which endpoints are available.

## Selection priority

Which endpoint is used is defined in this order:

- The endpoint given via command line flag `-e` or `--endpoint` is used if
  given.
- Otherwise, if given, the endpoint defined via the environment variable
  `GSCTL_ENDPOINT` is used.
- Otherwise the endpoint via `gsctl select endpoint` or `gsctl login` is used.

## Endpoint aliases {#alias}

To simplify the selection of an endpoint, each endpoint URL can have an alias.
When adding a new endpoint to your configuration by the use of
[`gsctl login`]({{< relref "/ui-api/gsctl/login" >}}), an alias for the endpoint is automatically set to
the unique name of the according Giant Swarm installation.

You can find out the alias of the currently selected endpoint, if set, using
[`gsctl info`]({{< relref "/ui-api/gsctl/info" >}})/). The command
[`gsctl list endpoints`]({{< relref "/ui-api/gsctl/list-endpoints" >}}) will print all available endpoints
and their aliases.

Aliases can be edited manually by editing your
[gsctl configuration file]({{< relref "/ui-api/gsctl/configuration-file" >}}). We
strongly recommend to use the same aliases throughout a team, as that
simplifies communication. Also note that aliases must be unique within your
configuration.

**Note:** Endpoint aliases have been added in gsctl version 0.10.0. Endpoints
added to your configuration by previous version don't have an alias set. A
simple way to add the default alias is to open your gsctl configuraiton in an
editor, then remove the endpoint entry without an alias, then use `gsctl login`
to log in with that endpoint URL again.

## Usage

```nohighlight
gsctl select endpoint <endpoint>
```

Example:

```nohighlight
gsctl select endpoint https://api.g8s.example.eu-central-1.aws.gigantic.io
```

A message will be printed letting you know if the endpoint has been selected.

With the alias `myalias` set for this endpoint, you could alternatively execute
that command:

```nohighlight
gsctl select endpoint myalias
```

## Related

- [`gsctl login`]({{< relref "/ui-api/gsctl/login" >}})
- [`gsctl list endpoints`]({{< relref "/ui-api/gsctl/list-endpoints" >}})
- [`gsctl delete endpoint`]({{< relref "/ui-api/gsctl/delete-endpoint" >}})
