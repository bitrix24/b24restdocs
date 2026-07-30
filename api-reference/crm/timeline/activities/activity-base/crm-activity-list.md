# Get List of Activities crm.activity.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `crm.activity.list` method returns a list of activities by filter, taking the current user's permissions into account.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields of the activity [crm.activity.fields](./crm-activity-fields.md) that need to be selected. To get the fields `COMMUNICATIONS` and `FILES`, specify them in select
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected items in key-value format.

Possible values for `field` correspond to the fields of the activity [crm.activity.fields](./crm-activity-fields.md).

An additional prefix can be assigned to the key to clarify the filter's behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as a value)
- `!@`— NOT IN (an array is passed as a value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values starting with "mol"
  - "%mol" — searching for values ending with "mol"
  - "%mol%" — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values not starting with "mol"
  - "%mol" — searching for values not ending with "mol"
  - "%mol%" — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal
||
|| **order**
[`object`](../../../data-types.md) | A set of key-value pairs for sorting the output results. The keys can use the fields of the activity [crm.activity.fields](./crm-activity-fields.md).

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

By default, it is sorted by increasing the Start Date field (`START_TIME`)
||
|| **start**
  [`integer`](../../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

See the description of [list methods](../../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% note info "" %}

Please note the specific behavior of the `filter[BINDINGS]` parameter.

An activity can be linked to multiple CRM entities. For example, a call can simultaneously be linked to a lead and a deal; therefore, to retrieve these entities, the `crm.activity.list` method parameters include a special filter key: `BINDINGS`.

You must specify an array of [system](../../../index.md) or [custom](../../../universal/user-defined-object-types/index.md) CRM object types for which you need to find a link.

Each object can consist of the `OWNER_TYPE_ID` keys (entity type identifier) and `OWNER_ID` keys (entity identifier), either individually or in combination. For example:

```json
"BINDINGS": [
    {"OWNER_TYPE_ID": 2},
    {"OWNER_TYPE_ID": 3}
]
```

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"OWNER_TYPE_ID":3,"OWNER_ID":102},"select":["*","COMMUNICATIONS"],"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"OWNER_TYPE_ID":3,"OWNER_ID":102},"select":["*","COMMUNICATIONS"],"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each activity item returned in result[]
    type ActivityItem = {
      ID: string
      OWNER_ID: string
      OWNER_TYPE_ID: string
      TYPE_ID: string
      SUBJECT: string
      CREATED: ISODate | null
      LAST_UPDATED: ISODate | null
      START_TIME: ISODate | null
      END_TIME: ISODate | null
      DEADLINE: ISODate | null
      COMPLETED: string
      STATUS: string
      RESPONSIBLE_ID: string
      DIRECTION: string
      AUTHOR_ID: string
      EDITOR_ID: string
    }

    try {
      // crm.activity.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ActivityItem[]>({
        method: 'crm.activity.list',
        params: {
          order: { ID: 'DESC' },
          filter: {
            OWNER_TYPE_ID: 3,
            OWNER_ID: 102,
          },
          select: ['*', 'COMMUNICATIONS'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activities on page:', result.length, result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listActivities() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.list',
            params: {
              order: { ID: 'DESC' },
              filter: {
                OWNER_TYPE_ID: 3,
                OWNER_ID: 102,
              },
              select: ['*', 'COMMUNICATIONS'],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activities on page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listActivities)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.list',
                [
                    'order' => [
                        'ID' => 'DESC',
                    ],
                    'filter' => [
                        'OWNER_TYPE_ID' => 3,
                        'OWNER_ID' => 102,
                    ],
                    'select' => [
                        '*',
                        'COMMUNICATIONS',
                    ],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Activities: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting activity list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.list',
        {
            order: {
                ID: 'DESC',
            },
            filter: {
                OWNER_TYPE_ID: 3,
                OWNER_ID: 102,
            },
            select: [
                '*',
                'COMMUNICATIONS',
            ],
            start: 0,
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'order' => [ 'ID' => 'DESC' ],
            'filter' => [
                'OWNER_TYPE_ID' => 3,
                'OWNER_ID' => 102
            ],
            'select' => [ '*', 'COMMUNICATIONS' ],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.list(
            select=["ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "STATUS", "DEADLINE", "RESPONSIBLE_ID"],
            filter={"OWNER_TYPE_ID": 2, "OWNER_ID": 101, "COMPLETED": "N"},
            order={"ID": "DESC"},
            start=0,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.list(
            select=["ID", "SUBJECT", "STATUS", "DEADLINE"],
            filter={"OWNER_TYPE_ID": 2, "OWNER_ID": 101},
            order={"ID": "ASC"},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list_fast`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.list(
            select=["ID", "SUBJECT", "STATUS", "DEADLINE"],
            filter={"OWNER_TYPE_ID": 2, "OWNER_ID": 101},
            order={"ID": "DESC"},
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](./crm-activity-list.md#example-bindings)
- [{#T}](./crm-activity-list.md#example-communications)
- [{#T}](./crm-activity-list.md#example-files)

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "20",
            "OWNER_ID": "15",
            "OWNER_TYPE_ID": "3",
            "TYPE_ID": "2",
            "PROVIDER_ID": "VOXIMPLANT_CALL",
            "PROVIDER_TYPE_ID": "CALL",
            "PROVIDER_GROUP_ID": null,
            "ASSOCIATED_ENTITY_ID": "0",
            "SUBJECT": "Outgoing call Klaus Weber",
            "CREATED": "2020-09-27T13:26:55+03:00",
            "LAST_UPDATED": "2021-03-21T20:28:24+03:00",
            "START_TIME": "2020-09-27T13:25:00+03:00",
            "END_TIME": "2020-09-27T19:25:00+03:00",
            "DEADLINE": "2020-09-27T13:25:00+03:00",
            "COMPLETED": "Y",
            "STATUS": "2",
            "RESPONSIBLE_ID": "505",
            "PRIORITY": "2",
            "NOTIFY_TYPE": "1",
            "NOTIFY_VALUE": "15",
            "DESCRIPTION": "",
            "DESCRIPTION_TYPE": "1",
            "DIRECTION": "2",
            "LOCATION": "",
            "SETTINGS": [],
            "ORIGINATOR_ID": null,
            "ORIGIN_ID": null,
            "AUTHOR_ID": "505",
            "EDITOR_ID": "505",
            "PROVIDER_PARAMS": [],
            "PROVIDER_DATA": null,
            "RESULT_MARK": "0",
            "RESULT_VALUE": null,
            "RESULT_SUM": null,
            "RESULT_CURRENCY_ID": null,
            "RESULT_STATUS": "0",
            "RESULT_STREAM": "0",
            "RESULT_SOURCE_ID": null,
            "AUTOCOMPLETE_RULE": "0"
        },
        // .. 49 more items
    ],
    "next": 50,
    "total": 123456,
    "time": {
        "start": 1724677896.295857,
        "finish": 1724677897.197243,
        "duration": 0.901386022567749,
        "processing": 0.8762130737304688,
        "date_start": "2024-08-26T16:11:36+03:00",
        "date_finish": "2024-08-26T16:11:37+03:00",
        "operating_reset_at": "2024-08-26T16:11:37+03:00",
        "operating": 0.0162130737304688
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | Array of activities. To get information about the activity structure, see the [crm.activity.fields](./crm-activity-fields.md) method ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "INVALID_REQUEST",
    "error_description": "Https required"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Specific Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

### Using BINGINDS {#example-bindings}

Retrieve fields: Identifier, Title, Owner Type (Entity Type Identifier), Owner (Entity Identifier)

Filter condition: the activity is linked to both a deal and a contact simultaneously

{% note info %}

When using multiple pairs in `BINDINGS`, duplication may occur in the results. For example, when executing the code example below, an activity linked to both entities will be output twice.

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"BINDINGS":[{"OWNER_TYPE_ID":2},{"OWNER_TYPE_ID":3}]},"select":["*","COMMUNICATIONS"],"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"BINDINGS":[{"OWNER_TYPE_ID":2},{"OWNER_TYPE_ID":3}]},"select":["*","COMMUNICATIONS"],"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each activity item returned in result[]
    type ActivityItem = {
      ID: string
      OWNER_ID: string
      OWNER_TYPE_ID: string
      TYPE_ID: string
      SUBJECT: string
      CREATED: ISODate | null
      LAST_UPDATED: ISODate | null
      START_TIME: ISODate | null
      END_TIME: ISODate | null
      DEADLINE: ISODate | null
      COMPLETED: string
      STATUS: string
      RESPONSIBLE_ID: string
      DIRECTION: string
      AUTHOR_ID: string
      EDITOR_ID: string
    }

    try {
      // crm.activity.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ActivityItem[]>({
        method: 'crm.activity.list',
        params: {
          order: { ID: 'DESC' },
          filter: {
            BINDINGS: [
              { OWNER_TYPE_ID: 2 },
              { OWNER_TYPE_ID: 3 },
            ],
          },
          select: ['*', 'COMMUNICATIONS'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activities matched by bindings on page:', result.length, result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listActivitiesWithBindings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.list',
            params: {
              order: { ID: 'DESC' },
              filter: {
                BINDINGS: [
                  { OWNER_TYPE_ID: 2 },
                  { OWNER_TYPE_ID: 3 },
                ],
              },
              select: ['*', 'COMMUNICATIONS'],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activities matched by bindings on page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listActivitiesWithBindings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.list',
                [
                    'order' => [
                        'ID' => 'DESC',
                    ],
                    'filter' => [
                        'BINDINGS' => [
                            [
                                'OWNER_TYPE_ID' => 2,
                            ],
                            [
                                'OWNER_TYPE_ID' => 3,
                            ],
                        ],
                    ],
                    'select' => [
                        '*',
                        'COMMUNICATIONS',
                    ],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Activities: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting activity list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.list',
        {
            order: {
                ID: 'DESC',
            },
            filter: {
                BINDINGS: [
                    {
                        OWNER_TYPE_ID: 2,
                    },
                    {
                        OWNER_TYPE_ID: 3,
                    },
                ],
            },
            select: [
                '*',
                'COMMUNICATIONS',
            ],
            start: 0,
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'order' => ['ID' => 'DESC'],
            'filter' => [
                'BINDINGS' => [
                    ['OWNER_TYPE_ID' => 2],
                    ['OWNER_TYPE_ID' => 3]
                ]
            ],
            'select' => ['*', 'COMMUNICATIONS'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Retrieving COMMUNICATIONS {#example-communications}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"20"},"select":["*","COMMUNICATIONS"],"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"20"},"select":["*","COMMUNICATIONS"],"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CommunicationEntry = {
      ID: string
      TYPE: string
      VALUE: string
      ENTITY_ID: string
      ENTITY_TYPE_ID: string
    }

    // Shape of each activity item returned in result[]
    type ActivityItem = {
      ID: string
      OWNER_ID: string
      OWNER_TYPE_ID: string
      SUBJECT: string
      CREATED: ISODate | null
      COMPLETED: string
      STATUS: string
      RESPONSIBLE_ID: string
      COMMUNICATIONS: CommunicationEntry[]
    }

    try {
      // crm.activity.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ActivityItem[]>({
        method: 'crm.activity.list',
        params: {
          filter: {
            ID: '20',
          },
          select: ['*', 'COMMUNICATIONS'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity ID:', result[0]?.ID, 'Communications:', result[0]?.COMMUNICATIONS)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listActivityCommunications() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.list',
            params: {
              filter: {
                ID: '20',
              },
              select: ['*', 'COMMUNICATIONS'],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity ID:', result[0]?.ID, 'Communications:', result[0]?.COMMUNICATIONS)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listActivityCommunications)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.list',
                [
                    'filter' => [
                        'ID' => '20',
                    ],
                    'select' => [
                        '*',
                        'COMMUNICATIONS',
                    ],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Activity communications: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting activity communications: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.list',
        {
            filter: {
                ID: '20',
            },
            select: [
                '*',
                'COMMUNICATIONS',
            ],
            start: 0,
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'ID' => '20'
            ],
            'select' => ['*', 'COMMUNICATIONS'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

#### Returned Data Example

HTTP status: **200**

```json
{
    "result": [
        {
        "ID": "20",
        "COMMUNICATIONS": [
            {
                "ID": "23",
                "TYPE": "PHONE",
                "VALUE": "499152222222",
                "ENTITY_ID": "15",
                "ENTITY_TYPE_ID": "3",
                "ENTITY_SETTINGS": {
                    "HONORIFIC": "1",
                    "NAME": "Klaus ",
                    "SECOND_NAME": "Weber",
                    "LAST_NAME": "",
                    "COMPANY_TITLE": "Müller GmbH",
                    "COMPANY_ID": "21"
                }
            }
        ]
    }
    ],
    "total": 1,
    "time": {
        "start": 1724659407.69855,
        "finish": 1724659407.723506,
        "duration": 0.02495598793029785,
        "processing": 0.003489971160888672,
        "date_start": "2024-08-26T11:03:27+03:00",
        "date_finish": "2024-08-26T11:03:27+03:00"
    }
}
```

### Retrieving Attachments {#example-files}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"101121"},"select":["*","STORAGE_ELEMENT_IDS"],"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"101121"},"select":["*","STORAGE_ELEMENT_IDS"],"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FileEntry = {
      id: number
      url: string
    }

    // Shape of each activity item returned in result[]
    type ActivityItem = {
      ID: string
      OWNER_ID: string
      OWNER_TYPE_ID: string
      SUBJECT: string
      CREATED: ISODate | null
      COMPLETED: string
      STATUS: string
      RESPONSIBLE_ID: string
      FILES: FileEntry[]
    }

    try {
      // crm.activity.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ActivityItem[]>({
        method: 'crm.activity.list',
        params: {
          filter: {
            ID: '101121',
          },
          select: ['*', 'STORAGE_ELEMENT_IDS'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity ID:', result[0]?.ID, 'Files:', result[0]?.FILES)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listActivityFiles() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.list',
            params: {
              filter: {
                ID: '101121',
              },
              select: ['*', 'STORAGE_ELEMENT_IDS'],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity ID:', result[0]?.ID, 'Files:', result[0]?.FILES)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listActivityFiles)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.list',
                [
                    'filter' => [
                        'ID' => '101121',
                    ],
                    'select' => [
                        '*',
                        'STORAGE_ELEMENT_IDS',
                    ],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Activity files: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting activity files: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.list',
        {
            filter: {
                ID: '101121',
            },
            select: [
                '*',
                'STORAGE_ELEMENT_IDS',
            ],
            start: 0,
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'ID' => '101121'
            ],
            'select' => ['*', 'STORAGE_ELEMENT_IDS'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

#### Returned Data Example

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "101121",
            "FILES": [
                {
                    "id": 3101820,
                    "url": "http://xxx.bitrix24.com/bitrix/tools/crm_show_file.php?fileId=3101820&ownerTypeId=6&ownerId=101121&auth="
                }
            ]
        }
    ],
    "total": 1,
    "time": {
        "start": 1724659652.591025,
        "finish": 1724659652.623784,
        "duration": 0.03275895118713379,
        "processing": 0.00624394416809082,
        "date_start": "2024-08-26T11:07:32+03:00",
        "date_finish": "2024-08-26T11:07:32+03:00"
    }
}
```

## Continue Learning

- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md)
- [{#T}](../../../../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md)
