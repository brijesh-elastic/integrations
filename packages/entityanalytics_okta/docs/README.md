# Okta Entity Analytics

This [Okta Entity Analytics](https://www.okta.com/) integration allows users to securely stream User Entities data to Elastic Security via the REST API. When integrated with Elastic Security, this valuable data can be leveraged within Elastic for risk-scoring scenarios (e.g., context enrichments) and detecting advanced analytics (UBA) use cases.

## Compatibility

This module has been tested against the Core Okta API version **v1**.

## Data streams

The Okta Entity Analytics integration collects one type of data: user.

**User** is used to retrieve all user logs available in an organization. See more details in the API documentation [here](https://developer.okta.com/docs/reference/api/users/#list-users).

## Requirements

- Elastic Agent must be installed.
- You can install only one Elastic Agent per host.
- Elastic Agent is required to stream data using Entity Analytics Input and ship the data to Elastic, where the events will then be processed via the integration's ingest pipelines.

### Installing and managing an Elastic Agent:

You have a few options for installing and managing an Elastic Agent:

### Install a Fleet-managed Elastic Agent (recommended):

With this approach, you install Elastic Agent and use Fleet in Kibana to define, configure, and manage your agents in a central location. We recommend using Fleet management because it makes the management and upgrade of your agents considerably easier.

### Install Elastic Agent in standalone mode (advanced users):

With this approach, you install Elastic Agent and manually configure the agent locally on the system where it’s installed. You are responsible for managing and upgrading the agents. This approach is reserved for advanced users only.

### Install Elastic Agent in a containerized environment:

You can run Elastic Agent inside a container, either with Fleet Server or standalone. Docker images for all versions of Elastic Agent are available from the Elastic Docker registry, and we provide deployment manifests for running on Kubernetes.

There are some minimum requirements for running Elastic Agent and for more information, refer to the link [here](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html).

The minimum **kibana.version** required is **8.9.0**.

## Setup

### To collect data from Okta, follow the below steps:

- Required URL namespace, which should be preceded by an organization's subdomain (tenant) or configured custom domain.
- Create an Okta API Token for Authentication. Follow this [guide](https://developer.okta.com/docs/guides/create-an-api-token/main/).

### Enabling the integration in Elastic:

1. In Kibana, go to Management > Integrations.
2. In the "Search for integrations" search bar, type Okta Entity Analytics.
3. Click on the "Okta Entity Analytics" integration from the search results.
4. Click on the Add Okta Entity Analytics Integration button to add the integration.
5. While adding the integration, add the URL and API Token that we got earlier.
6. Save the integration by adding other necessary parameters.

## Usage

The Okta provider periodically contacts the Okta API, retrieving updates for users, updates its internal cache of user metadata, and ships updated user metadata to Elasticsearch.

Fetching and shipping updates occurs in one of two processes: **full synchronizations** and **incremental updates**. Full synchronizations will send the entire list of users in state, along with write markers to indicate the start and end of the synchronization event. Incremental updates will only send data for changed users during that event. Changes on a user can come in many forms, whether it be a change to the user’s metadata, or a user was added or deleted. By default, full synchronizations occur every 24 hours and incremental updates occur every 15 minutes. These intervals may be customized to suit your use case.

## Sample Events

A user document:

```json
{
  "@timestamp": "2023-07-04T09:57:19.786056-05:00",
  "event": {
    "action": "user-discovered"
  },
  "okta": {
    "id": "userid",
    "status": "RECOVERY",
    "created": "2023-06-02T09:33:00.189752+09:30",
    "activated": "0001-01-01T00:00:00Z",
    "statusChanged": "2023-06-02T09:33:00.189752+09:30",
    "lastLogin": "2023-06-02T09:33:00.189752+09:30",
    "lastUpdated": "2023-06-02T09:33:00.189753+09:30",
    "passwordChanged": "2023-06-02T09:33:00.189753+09:30",
    "type": {
      "id": "typeid"
    },
    "profile": {
      "login": "name.surname@example.com",
      "email": "name.surname@example.com",
      "firstName": "name",
      "lastName": "surname"
    },
    "credentials": {
      "password": {},
      "provider": {
        "type": "OKTA",
        "name": "OKTA"
      }
    },
    "_links": {
      "self": {
        "href": "https://localhost/api/v1/users/userid"
      }
    }
  },
  "user": {
    "id": "userid"
  },
  "labels": {
    "identity_source": "okta-1"
  }
}
```

Full synchronizations will be bounded on either side by "write marker" documents.

```json
{
    "@timestamp": "2022-11-04T09:57:19.786056-05:00",
    "event": {
        "action": "started",
        "start": "2022-11-04T09:57:19.786056-05:00"
    },
    "labels": {
        "identity_source": "okta-1"
    }
}
```

## Logs reference

### User

This is the `User` dataset.

#### Example

An example event for `user` looks as following:

```json
{
    "@timestamp": "2023-07-03T07:27:23.714Z",
    "agent": {
        "ephemeral_id": "3b46c799-4885-4629-8199-73092efacf18",
        "id": "bcc7c06a-3869-49ec-8729-13e94533da42",
        "name": "docker-fleet-agent",
        "type": "filebeat",
        "version": "8.9.0"
    },
    "asset": {
        "create_date": "2013-06-24T16:39:18.000Z",
        "last_seen": "2013-06-24T17:39:19.000Z",
        "last_status_change_date": "2013-06-24T16:39:19.000Z",
        "last_updated": "2013-07-02T21:36:25.344Z",
        "status": "ACTIVE",
        "vendor": "OKTA"
    },
    "data_stream": {
        "dataset": "entityanalytics_okta.user",
        "namespace": "ep",
        "type": "logs"
    },
    "ecs": {
        "version": "8.8.0"
    },
    "elastic_agent": {
        "id": "bcc7c06a-3869-49ec-8729-13e94533da42",
        "snapshot": true,
        "version": "8.9.0"
    },
    "entityanalytics_okta": {
        "user": {
            "activated": "2013-06-24T16:39:19.000Z",
            "created": "2013-06-24T16:39:18.000Z",
            "credentials": {
                "provider": {
                    "name": "OKTA",
                    "type": "OKTA"
                }
            },
            "id": "00ub0oNGTSWTBKOLGLNR",
            "last_login": "2013-06-24T17:39:19.000Z",
            "last_updated": "2013-07-02T21:36:25.344Z",
            "password_changed": "2013-07-02T21:36:25.344Z",
            "profile": {
                "email": "isaac.brock@example.com",
                "first_name": "Isaac",
                "last_name": "Brock",
                "login": "isaac.brock@example.com",
                "mobile_phone": "555-415-1337"
            },
            "status": "ACTIVE",
            "status_changed": "2013-06-24T16:39:19.000Z"
        }
    },
    "event": {
        "action": "user-discovered",
        "agent_id_status": "verified",
        "category": [
            "iam"
        ],
        "dataset": "entityanalytics_okta.user",
        "ingested": "2023-07-03T07:27:26Z",
        "kind": "asset",
        "type": [
            "user",
            "info"
        ]
    },
    "input": {
        "type": "entity-analytics"
    },
    "labels": {
        "identity_source": "entity-analytics-entityanalytics_okta.user-c2d7bccc-9a82-4bf2-885d-46ac21caa1f7"
    },
    "related": {
        "user": [
            "00ub0oNGTSWTBKOLGLNR",
            "isaac.brock@example.com",
            "Isaac",
            "Brock"
        ]
    },
    "tags": [
        "preserve_duplicate_custom_fields",
        "forwarded",
        "entityanalytics_okta-user"
    ],
    "user": {
        "account": {
            "activated_date": "2013-06-24T16:39:19.000Z",
            "change_date": "2013-06-24T16:39:19.000Z",
            "create_date": "2013-06-24T16:39:18.000Z",
            "password_change_date": "2013-07-02T21:36:25.344Z",
            "status": {
                "deprovisioned": false,
                "locked_out": false,
                "password_expired": false,
                "recovery": false,
                "suspended": false
            }
        },
        "email": "isaac.brock@example.com",
        "id": "00ub0oNGTSWTBKOLGLNR",
        "name": "isaac.brock@example.com",
        "profile": {
            "first_name": "Isaac",
            "last_name": "Brock",
            "mobile_phone": "555-415-1337",
            "status": "ACTIVE"
        }
    }
}
```

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| asset.costCenter |  | keyword |
| asset.create_date |  | date |
| asset.last_seen |  | date |
| asset.last_status_change_date |  | date |
| asset.last_updated |  | date |
| asset.name |  | keyword |
| asset.status |  | keyword |
| asset.vendor |  | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| entityanalytics_okta.user._embedded | embedded resources related to the user. | object |
| entityanalytics_okta.user._links | link relations for the user's current status. | object |
| entityanalytics_okta.user.activated | timestamp when transition to ACTIVE status completed. | date |
| entityanalytics_okta.user.created | timestamp when user was created. | date |
| entityanalytics_okta.user.credentials.password.hash.algorithm | The algorithm used to generate the hash using the password (and salt, when applicable). Must be set to BCRYPT, SHA-512, SHA-256, SHA-1, MD5 or PBKDF2. | keyword |
| entityanalytics_okta.user.credentials.password.hash.digest_algorithm | Algorithm used to generate the key. Currently we support "SHA256_HMAC" and "SHA512_HMAC“. Only required for PBKDF2 algorithm. | keyword |
| entityanalytics_okta.user.credentials.password.hash.iteration_count | The number of iterations used when hashing passwords using PBKDF2. Must be \>= 4096. Only required for PBKDF2 algorithm. | long |
| entityanalytics_okta.user.credentials.password.hash.key_size | Size of the derived key in bytes. Only required for PBKDF2 algorithm. | long |
| entityanalytics_okta.user.credentials.password.hash.salt | Only required for salted hashes. For BCRYPT, this specifies the radix64-encoded salt used to generate the hash, which must be 22 characters long. For other salted hashes, this specifies the base64-encoded salt used to generate the hash. | keyword |
| entityanalytics_okta.user.credentials.password.hash.salt_order | Specifies whether salt was pre- or postfixed to the password before hashing. Only required for salted algorithms. | keyword |
| entityanalytics_okta.user.credentials.password.hash.value | For SHA-512, SHA-256, SHA-1, MD5 and PBKDF2, This is the actual base64-encoded hash of the password (and salt, if used). This is the Base64 encoded value of the SHA-512/SHA-256/SHA-1/MD5/PBKDF2 digest that was computed by either pre-fixing or post-fixing the salt to the password, depending on the saltOrder. If a salt was not used in the source system, then this should just be the the Base64 encoded value of the password's SHA-512/SHA-256/SHA-1/MD5/PBKDF2 digest. For BCRYPT, This is the actual radix64-encoded hashed password. | keyword |
| entityanalytics_okta.user.credentials.password.hash.work_factor | Governs the strength of the hash and the time required to compute it. Only required for BCRYPT algorithm. Minimum value is 1, and maximum is 20. | long |
| entityanalytics_okta.user.credentials.password.hook.type | The type of password inline hook. Currently, must be set to default. | keyword |
| entityanalytics_okta.user.credentials.password.value |  | keyword |
| entityanalytics_okta.user.credentials.provider.name |  | keyword |
| entityanalytics_okta.user.credentials.provider.type |  | keyword |
| entityanalytics_okta.user.credentials.recovery_question.answer |  | keyword |
| entityanalytics_okta.user.credentials.recovery_question.question |  | keyword |
| entityanalytics_okta.user.id | unique key for user. | keyword |
| entityanalytics_okta.user.last_login | timestamp of last login. | date |
| entityanalytics_okta.user.last_updated | timestamp when user was last updated. | date |
| entityanalytics_okta.user.password_changed | timestamp when password last changed. | date |
| entityanalytics_okta.user.profile.city | City or locality component of user's address (locality). | keyword |
| entityanalytics_okta.user.profile.cost_center | Name of a cost center assigned to user. | keyword |
| entityanalytics_okta.user.profile.country_code | Country name component of user's address (country). | keyword |
| entityanalytics_okta.user.profile.department | Name of user's department. | keyword |
| entityanalytics_okta.user.profile.display_name | Name of the user, suitable for display to end users. | keyword |
| entityanalytics_okta.user.profile.division | Name of user's division. | keyword |
| entityanalytics_okta.user.profile.email | Primary email address of user. | keyword |
| entityanalytics_okta.user.profile.employee_number | Organization or company assigned unique identifier for the user. | keyword |
| entityanalytics_okta.user.profile.first_name | Given name of the user (givenName). | keyword |
| entityanalytics_okta.user.profile.honorific.prefix | Honorific prefix(es) of the user, or title in most Western languages. | keyword |
| entityanalytics_okta.user.profile.honorific.suffix | Honorific suffix(es) of the user. | keyword |
| entityanalytics_okta.user.profile.last_name | Family name of the user (familyName). | keyword |
| entityanalytics_okta.user.profile.locale | User's default location for purposes of localizing items such as currency, date time format, numerical representations, and so on. | keyword |
| entityanalytics_okta.user.profile.login | Unique identifier for the user (username). | keyword |
| entityanalytics_okta.user.profile.manager.id | id of a user's manager. | keyword |
| entityanalytics_okta.user.profile.manager.name | displayName of the user's manager. | keyword |
| entityanalytics_okta.user.profile.middle_name | Middle name(s) of the user. | keyword |
| entityanalytics_okta.user.profile.mobile_phone | Mobile phone number of user. | keyword |
| entityanalytics_okta.user.profile.nick_name | Casual way to address the user in real life. | keyword |
| entityanalytics_okta.user.profile.organization | Name of user's organization. | keyword |
| entityanalytics_okta.user.profile.postal_address | Mailing address component of user's address. | keyword |
| entityanalytics_okta.user.profile.preferred_language | User's preferred written or spoken languages. | keyword |
| entityanalytics_okta.user.profile.primary_phone | Primary phone number of user such as home number. | keyword |
| entityanalytics_okta.user.profile.second_email | Secondary email address of user typically used for account recovery. | keyword |
| entityanalytics_okta.user.profile.state | State or region component of user's address (region). | keyword |
| entityanalytics_okta.user.profile.street_address | Full street address component of user's address. | keyword |
| entityanalytics_okta.user.profile.timezone | User's time zone. | keyword |
| entityanalytics_okta.user.profile.title | User's title, such as "Vice President". | keyword |
| entityanalytics_okta.user.profile.url | URL of user's online profile (for example: a web page). | keyword |
| entityanalytics_okta.user.profile.user_type | Used to describe the organization to user relationship such as "Employee" or "Contractor". | keyword |
| entityanalytics_okta.user.profile.zip_code | ZIP code or postal code component of user's address (postalCode). | keyword |
| entityanalytics_okta.user.status | current status of user. | keyword |
| entityanalytics_okta.user.status_changed | timestamp when status last changed. | date |
| entityanalytics_okta.user.transitioning_to_status | target status of an in-progress asynchronous status transition. | keyword |
| entityanalytics_okta.user.type | user type that determines the schema for the user's profile. | object |
| event.dataset | Event dataset. | constant_keyword |
| event.module | Event module. | constant_keyword |
| input.type | Type of filebeat input. | keyword |
| labels.identity_source |  | keyword |
| log.offset | Log offset. | long |
| tags | User defined tags. | keyword |
| user.account.activated_date |  | date |
| user.account.change_date |  | date |
| user.account.create_date |  | date |
| user.account.password_change_date |  | date |
| user.account.status.deprovisioned |  | boolean |
| user.account.status.locked_out |  | boolean |
| user.account.status.password_expired |  | boolean |
| user.account.status.recovery |  | boolean |
| user.account.status.suspended |  | boolean |
| user.profile.department |  | keyword |
| user.profile.first_name |  | keyword |
| user.profile.id |  | keyword |
| user.profile.job_title |  | keyword |
| user.profile.last_name |  | keyword |
| user.profile.manager |  | keyword |
| user.profile.mobile_phone |  | keyword |
| user.profile.organization |  | keyword |
| user.profile.other_identities |  | keyword |
| user.profile.primaryPhone |  | keyword |
| user.profile.secondEmail |  | keyword |
| user.profile.status |  | keyword |
| user.profile.type |  | keyword |
