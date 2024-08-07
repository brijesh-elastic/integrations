# Redis

## Metrics

The `redis` dataset fetches metrics from [GCP Memorystore](https://cloud.google.com/memorystore/) for [Redis](https://cloud.google.com/memorystore/) in Google Cloud Platform. It contains all metrics exported from the [GCP Redis Monitoring API](https://cloud.google.com/monitoring/api/metrics_gcp#gcp-redis).

## Sample Event
    
An example event for `redis` looks as following:

```json
{
    "@timestamp": "2017-10-12T08:05:34.853Z",
    "cloud": {
        "account": {
            "id": "elastic-obs-integrations-dev",
            "name": "elastic-obs-integrations-dev"
        },
        "instance": {
            "id": "4751091017865185079",
            "name": "gke-cluster-1-default-pool-6617a8aa-5clh"
        },
        "machine": {
            "type": "e2-medium"
        },
        "provider": "gcp",
        "availability_zone": "us-central1-c",
        "region": "us-central1"
    },
    "event": {
        "dataset": "gcp.redis",
        "duration": 115000,
        "module": "gcp"
    },
    "gcp": {
        "redis": {
            "clients": {
                "blocked": {
                    "count": 4
                }
            }
        },
        "labels": {
            "user": {
                "goog-gke-node": ""
            }
        }
    },
    "host": {
        "id": "4751091017865185079",
        "name": "gke-cluster-1-default-pool-6617a8aa-5clh"
    },
    "metricset": {
        "name": "metrics",
        "period": 10000
    },
    "service": {
        "type": "gcp"
    }
}
```

## Exported fields

**ECS Field Reference**

Please refer to the following [document](https://www.elastic.co/guide/en/ecs/current/ecs-field-reference.html) for detailed information on ECS fields.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| agent.id | Unique identifier of this agent (if one exists). Example: For Beats this would be beat.id. | keyword |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| gcp.labels.metadata.\* |  | object |  |  |
| gcp.labels.metrics.\* |  | object |  |  |
| gcp.labels.resource.\* |  | object |  |  |
| gcp.labels.system.\* |  | object |  |  |
| gcp.labels.user.\* |  | object |  |  |
| gcp.labels_fingerprint | Hashed value of the labels field. | keyword |  |  |
| gcp.metrics.\*.\*.\*.\* | Metrics that returned from Google Cloud API query. | object |  |  |
| gcp.redis.clients.blocked.count | Number of blocked clients. | long |  | gauge |
| gcp.redis.clients.connected.count | Number of client connections. | long |  | gauge |
| gcp.redis.commands.calls.count | Delta of the number of calls for this command in one minute. | long |  | gauge |
| gcp.redis.commands.total_time.us | Delta of the amount of time in microseconds that this command took in the last second. | long | micros | gauge |
| gcp.redis.commands.usec_per_call.sec | Average time per call over 1 minute by command. | double | s | gauge |
| gcp.redis.keyspace.avg_ttl.sec | Average TTL for keys in this database. | double | s | gauge |
| gcp.redis.keyspace.keys.count | Number of keys stored in this database. | long |  | gauge |
| gcp.redis.keyspace.keys_with_expiration.count | Number of keys with an expiration in this database. | long |  | gauge |
| gcp.redis.persistence.rdb.bgsave_in_progress | Flag indicating a RDB save is on-going. | boolean |  |  |
| gcp.redis.replication.master.slaves.lag.sec | The number of seconds that replica is lagging behind primary. | long | s | gauge |
| gcp.redis.replication.master.slaves.offset.bytes | The number of bytes that have been acknowledged by replicas. | long | byte | gauge |
| gcp.redis.replication.master_repl_offset.bytes | The number of bytes that master has produced and sent to replicas. | long | byte | gauge |
| gcp.redis.replication.offset_diff.bytes | The largest number of bytes that have not been replicated across all replicas. This is the biggest difference between replication byte offset (master) and replication byte offset (replica) of all replicas. | long | byte | gauge |
| gcp.redis.replication.role | Returns a value indicating the node role. 1 indicates primary and 0 indicates replica. | long |  | gauge |
| gcp.redis.server.uptime.sec | Uptime in seconds. | long | s | gauge |
| gcp.redis.stats.cache_hit_ratio | Cache Hit ratio as a fraction. | double |  | gauge |
| gcp.redis.stats.connections.total.count | Delta of the total number of connections accepted by the server. | long |  | gauge |
| gcp.redis.stats.cpu_utilization.sec | CPU-seconds consumed by the Redis server, broken down by system/user space and parent/child relationship. | double | s | gauge |
| gcp.redis.stats.evicted_keys.count | Delta of the number of evicted keys due to maxmemory limit. | long |  | gauge |
| gcp.redis.stats.expired_keys.count | Delta of the total number of key expiration events. | long |  | gauge |
| gcp.redis.stats.keyspace_hits.count | Delta of the number of successful lookup of keys in the main dictionary. | long |  | gauge |
| gcp.redis.stats.keyspace_misses.count | Delta of the number of failed lookup of keys in the main dictionary. | long |  | gauge |
| gcp.redis.stats.memory.maxmemory.mb | Maximum amount of memory Redis can consume. | long | m | gauge |
| gcp.redis.stats.memory.system_memory_overload_duration.us | The amount of time in microseconds the instance is in system memory overload mode. | long | micros | gauge |
| gcp.redis.stats.memory.system_memory_usage_ratio | Memory usage as a ratio of maximum system memory. | double |  | gauge |
| gcp.redis.stats.memory.usage.bytes | Total number of bytes allocated by Redis. | long | byte | gauge |
| gcp.redis.stats.memory.usage_ratio | Memory usage as a ratio of maximum memory. | double |  | gauge |
| gcp.redis.stats.network_traffic.bytes | Delta of the total number of bytes sent to/from redis (includes bytes from commands themselves, payload data, and delimiters). | long | byte | gauge |
| gcp.redis.stats.pubsub.channels.count | Global number of pub/sub channels with client subscriptions. | long |  | gauge |
| gcp.redis.stats.pubsub.patterns.count | Global number of pub/sub pattern with client subscriptions. | long |  | gauge |
| gcp.redis.stats.reject_connections.count | Number of connections rejected because of maxclients limit. | long |  | gauge |
| host.containerized | If the host is a container. | boolean |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |

