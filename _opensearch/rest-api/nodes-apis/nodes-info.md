---
layout: default
title: Nodes info
parent: Nodes APIs
grand_parent: REST API reference
nav_order: 20
---

# Nodes info

Represents mostly static information about your cluster's nodes, including but not limited to:

- Host system information 
- JVM 
- Processor Type 
- Node settings 
- Thread pools settings 
- Installed plugins

## Example

To get information on all nodes in a cluster:

```bash
GET /_nodes
```

To get thread pool information from the cluster manager node only:

```bash
GET /_nodes/master:true/thread_pool?pretty
```

## Path and HTTP methods

```bash
GET /_nodes
GET /_nodes/{nodeId}
GET /_nodes/{metrics}
GET /_nodes/{nodeId}/{metrics}
# or full path equivalent
GET /_nodes/{nodeId}/info/{metrics}
```

## URL parameters

You can include the following URL parameters in your request. All parameters are optional.

Parameter | Type   | Description
:--- |:-------| :---
nodeId | String | A comma-separated list of nodeIds to filter results. Supports [node filters]({{site.url}}{{site.baseurl}}/opensearch/rest-api/nodes-apis/index/#node-filters). Defaults to `_all`.
metrics | String | A comma-separated list of metric groups that will be included in the response. For example `jvm,thread_pools`. Defaults to all metrics.
timeout | TimeValue | A request [timeout]({{site.url}}{{site.baseurl}}/opensearch/rest-api/nodes-apis/index/#timeout). Defaults to `30s`.

The following are listed for all available metric groups:

Metric | Description
:--- |:----
`settings` | A node's settings. This is combination of the default settings, custom settings from [configuration file]({{site.url}}{{site.baseurl}}/opensearch/configuration/#configuration-file) and dynamically [updated settings]({{site.url}}{{site.baseurl}}/opensearch/configuration/#update-cluster-settings-using-the-api).
`os` | Static information about the host OS, including version, processor architecture and available/allocated processors.
`process` | Contains process OD.
`jvm` | Detailed static information about running JVM, including arguments.
`thread_pool` | Configured options for all individual thread pools.
`transport` | Mostly static information about the transport layer.
`http` | Mostly static information about the HTTP layer.
`plugins` | Information about installed plugins and modules.
`ingest` | Information about ingest pipelines and available ingest processors.
`aggregations` | Information about available [aggregations]({{site.url}}{{site.baseurl}}/opensearch/aggregations).
`indices` | Static index settings configured at the node level.

## Response

The response contains basic node identification and build info for every node
matching the `{nodeId}` request parameter.

Metric | Description
:--- |:----
name | A node name.
transport_address | A node transport address.
host | A node host address.
ip | A node host ip address.
version | A node OpenSearch version.
build_type | A build type, like `rpm`,`docker`, `zip`, etc.
build_hash | A git commit hash of the build.
roles | A node's role.
attributes | A node attributes.

The response also contains one or more metric groups, depending on the `{metrics}` request parameter.

```json
GET /_nodes/master:true/process,transport?pretty

{
  "_nodes": {
    "total": 1,
    "successful": 1,
    "failed": 0
  },
  "cluster_name": "opensearch",
  "nodes": {
    "VC0d4RgbTM6kLDwuud2XZQ": {
      "name": "node-m1-23",
      "transport_address": "127.0.0.1:9300",
      "host": "127.0.0.1",
      "ip": "127.0.0.1",
      "version": "1.3.1",
      "build_type": "tar",
      "build_hash": "c4c0672877bf0f787ca857c7c37b775967f93d81",
      "roles": [
        "data",
        "ingest",
        "master",
        "remote_cluster_client"
      ],
      "attributes": {
        "shard_indexing_pressure_enabled": "true"
      },
      "process" : {
        "refresh_interval_in_millis": 1000,
        "id": 44584,
        "mlockall": false
      },
      "transport": {
        "bound_address": [
          "[::1]:9300",
          "127.0.0.1:9300"
        ],
        "publish_address": "127.0.0.1:9300",
        "profiles": { }
      }
    }
  }
}
```

## Required permissions

If you use the security plugin, make sure you have the appropriate permissions: `cluster:monitor/nodes/info`.
