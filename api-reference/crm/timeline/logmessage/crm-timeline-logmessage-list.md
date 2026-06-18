# Get a List of Timeline Log Entries crm.timeline.logmessage.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `user with read access permission for the CRM entity containing the record`

This method retrieves a list of timeline log entries.

{% note info "" %}

It is important to note that the method can only retrieve data for entries that were previously added using [`crm.timeline.logmessage.add`](./crm-timeline-logmessage-add.md). System entries cannot be retrieved using `crm.timeline.logmessage.list`.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`](../../../data-types.md) | [Identifier of the entity type](../../data-types.md#object_type) for which to retrieve the list of log entries (e.g., `1` — lead) ||
|| **entityId*** 
[`integer`](../../../data-types.md) | Identifier of the entity item for which to retrieve the list of log entries (e.g., `1`) ||
|| **order** 
[`object`](../../../data-types.md) | List for sorting, where the key is the field and the value is `asc` or `desc`.

By default, `desc` is used.

Sorting is supported only by the **id** and **created** fields. ||
|| **start** 
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 10 entries.

To select the second page of results, you must pass the value `10`. To select the third page of results — the value `20`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N - 1) * 10`, where `N` is the desired page number. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"entityId":1,"order":{"created":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.logmessage.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"entityId":1,"order":{"created":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.logmessage.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each log message returned in result[]
    type LogMessageItem = {
      id: number
      created: ISODate
      authorId: number
      title: string
      text: string
      iconCode: string
    }

    try {
      // crm.timeline.logmessage.list returns a single page (max 10 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<LogMessageItem[]>({
        method: 'crm.timeline.logmessage.list',
        params: {
          entityTypeId: 1,
          entityId: 1,
          order: { created: 'desc' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Log messages:', result.length, result[0]?.title)
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
      async function listLogMessages() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.timeline.logmessage.list returns a single page (max 10 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.logmessage.list',
            params: {
              entityTypeId: 1,
              entityId: 1,
              order: { created: 'desc' },
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
          console.info('Log messages:', result.length, result[0]?.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listLogMessages)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.timeline.logmessage.list',
                [
                    'entityTypeId' => 1,
                    'entityId'     => 1,
                    'order'        => ['created' => 'desc'],
                    'start'        => 0,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing log messages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.logmessage.list",
        {
            entityTypeId: 1,
            entityId: 1,
            order: { created: "desc" },
        },
        result => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.dir(result.data());

            if (result.more()) {
                result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.logmessage.list',
        [
            'entityTypeId' => 1,
            'entityId' => 1,
            'order' => ['created' => 'desc'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "id": 42074,
            "created": "2024-04-03T10:26:32+02:00",
            "authorId": 1,
            "title": "Test title new",
            "text": "Test note new",
            "iconCode": "info"
        },
        {
            "id": 42073,
            "created": "2024-04-03T10:26:32+02:00",
            "authorId": 1,
            "title": "Test title",
            "text": "Test note",
            "iconCode": "info"
        }
    ],
    "total": 2,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | The root element of the response.

The `result` field contains an array, each entry of which contains an associative array of log entry fields [logMessage](./crm-timeline-logmessage-add.md#logMessage) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {entityTypeId}"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `100` | Required fields are missing ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-logmessage-add.md)
- [{#T}](./crm-timeline-logmessage-get.md)
- [{#T}](./crm-timeline-logmessage-delete.md)