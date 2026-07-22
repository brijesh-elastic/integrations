# Lumos Integration

The Lumos integration uses [Lumos' API](https://www.lumos.com/) to retrieve Activity Logs and ingest them into Elasticsearch. This allows you to search, observe, and visualize the Activity Logs through Elasticsearch.

The Elastic agent running this integration interacts with Lumos' infrastructure using their APIs to retrieve Activity Logs for a Lumos tenant.

## Agentless Enabled Integration

Agentless integrations allow you to collect data without having to manage Elastic Agent in your cloud. They make manual agent deployment unnecessary, so you can focus on your data instead of the agent that collects it. For more information, refer to [Agentless integrations](https://www.elastic.co/guide/en/serverless/current/security-agentless-integrations.html) and the [Agentless integrations FAQ](https://www.elastic.co/guide/en/serverless/current/agentless-integration-troubleshooting.html).
Agentless deployments are only supported in Elastic Serverless and Elastic Cloud environments.  This functionality is in beta and is subject to change. Beta features are not subject to the support SLA of official GA features.

## Configuration

### Enabling the integration in Elastic

1. In Kibana go to **Management > Integrations**
2. In the "Search for integrations" search bar type **Lumos**.
3. Click on "Lumos" integration from the search results.
4. Click on **Add Lumos** button to add Lumos integration.

### Configure Lumos Activity Logs data stream

1. In Lumos go to **Settings > API Tokens**
2. Click on "Add API Token", enter a name and description
3. Copy the key starting with `lsk_`
4. While adding Lumos integration in Elastic, paste your key into the `API Token` field

## Logs

### Activity Logs

Activity Logs summarize the history of changes and events occurring within Lumos.

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| event.module | Event module | constant_keyword |
| input.type | Input type | keyword |
| lumos.activity_logs.actor.actor_type | The type of actor | keyword |
| lumos.activity_logs.actor.email | The email of the actor | keyword |
| lumos.activity_logs.actor.family_name | The family name of the actor | keyword |
| lumos.activity_logs.actor.given_name | The given name of the actor | keyword |
| lumos.activity_logs.event_began_at | The time the event began | keyword |
| lumos.activity_logs.event_type_user_friendly | The user friendly type of the event | keyword |
| lumos.activity_logs.targets.name |  | keyword |
| lumos.activity_logs.targets.target_type |  | keyword |


An example event for `activity` looks as following:

```json
{
    "@timestamp": "2026-07-22T09:40:48.477Z",
    "agent": {
        "ephemeral_id": "9e24ed1c-c73a-41e6-a63f-dd1026e4884f",
        "id": "4912077b-b7ff-4757-a721-3ad4ab97fead",
        "name": "elastic-agent-48882",
        "type": "filebeat",
        "version": "8.19.4"
    },
    "data_stream": {
        "dataset": "lumos.activity_logs",
        "namespace": "91086",
        "type": "logs"
    },
    "ecs": {
        "version": "8.11.0"
    },
    "elastic_agent": {
        "id": "4912077b-b7ff-4757-a721-3ad4ab97fead",
        "snapshot": false,
        "version": "8.19.4"
    },
    "event": {
        "action": "SOD_POLICY_DELETED",
        "agent_id_status": "verified",
        "created": "2026-07-22T09:40:48.477Z",
        "dataset": "lumos.activity_logs",
        "id": "1111111111111111111111111111111111111111111111111111111111111111",
        "ingested": "2026-07-22T09:40:51Z",
        "kind": "event",
        "outcome": "success",
        "type": [
            "info"
        ]
    },
    "host": {
        "architecture": "aarch64",
        "containerized": false,
        "hostname": "elastic-agent-48882",
        "ip": [
            "172.19.0.2",
            "172.18.0.4"
        ],
        "mac": [
            "26-E9-8B-78-AF-7F",
            "E6-AD-7E-E1-1B-0B"
        ],
        "name": "elastic-agent-48882",
        "os": {
            "kernel": "6.12.76-linuxkit",
            "name": "Wolfi",
            "platform": "wolfi",
            "type": "linux",
            "version": "20230201"
        }
    },
    "input": {
        "type": "httpjson"
    },
    "lumos": {
        "activity_logs": {
            "actor": {
                "actor_type": "Lumos user",
                "email": "wile.e.coyote@example.com",
                "family_name": "Coyote",
                "given_name": "Wile"
            },
            "event_began_at": "2024-03-12T16:09:14",
            "event_type_user_friendly": "A user deleted a SOD Policy",
            "targets": [
                {
                    "name": "Untitled Rule",
                    "target_type": "SOD Policy"
                }
            ]
        }
    },
    "message": "{\"actor\":{\"actor_type\":\"Lumos user\",\"email\":\"wile.e.coyote@example.com\",\"family_name\":\"Coyote\",\"given_name\":\"Wile\"},\"event_began_at\":\"2024-03-12T16:09:14\",\"event_hash\":\"1111111111111111111111111111111111111111111111111111111111111111\",\"event_metadata\":{},\"event_type\":\"SOD_POLICY_DELETED\",\"event_type_user_friendly\":\"A user deleted a SOD Policy\",\"outcome\":\"Succeeded\",\"targets\":[{\"name\":\"Untitled Rule\",\"target_type\":\"SOD Policy\"}]}"
}
```