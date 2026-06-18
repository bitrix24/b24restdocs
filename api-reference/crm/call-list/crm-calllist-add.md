# Create a New Call List crm.calllist.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with access permission to read CRM entities

The method `crm.calllist.add` creates a new call list.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE***
[`string`](../../data-types.md) | Type of the object: 
- `CONTACT` — contact,
- `COMPANY` — company ||
|| **ENTITIES***
[`array`](../../data-types.md) | Array of `ID` of contacts or companies, which can be obtained using the [crm.item.list](../universal/crm-item-list.md) method ||
|| **WEBFORM_ID**
[`integer`](../../data-types.md) | `ID` of the CRM form that will be displayed in the call list form. 
The `ID` can be found in the list of forms in Bitrix24 https://your-domain.com/crm/webform/ ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5,"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.calllist.add',
        params: {
          ENTITY_TYPE: 'CONTACT',
          ENTITIES: [9, 17, 19],
          WEBFORM_ID: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created call list ID:', result)
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
      async function addCallList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.calllist.add',
            params: {
              ENTITY_TYPE: 'CONTACT',
              ENTITIES: [9, 17, 19],
              WEBFORM_ID: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created call list ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCallList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.calllist.add',
                [
                    'ENTITY_TYPE' => 'CONTACT',
                    'ENTITIES'    => [9, 17, 19],
                    'WEBFORM_ID'  => 1,
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
        echo 'Error adding call list: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.calllist.add(
            entity_type="CONTACT",
            entities=[9, 17, 19],
            webform_id=1,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.calllist.add",
        {
            ENTITY_TYPE: "CONTACT",
            ENTITIES: [9,17,19],
            WEBFORM_ID: 1
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.calllist.add',
        [
            'ENTITY_TYPE' => 'CONTACT',
            'ENTITIES' => [1,2,3],
            'WEBFORM_ID' => 5
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 11,
    "time": {
        "start": 1752471668.062547,
        "finish": 1752471668.531711,
        "duration": 0.4691641330718994,
        "processing": 0.4174520969390869,
        "date_start": "2025-07-14T08:41:08+02:00",
        "date_finish": "2025-07-14T08:41:08+02:00",
        "operating_reset_at": 1752472268,
        "operating": 0.41742897033691406
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | ID of the created call list ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Invalid parameters.",
    "error_description": "Invalid parameters were provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Access denied` | No permission to perform the operation ||
|| `400` | `Invalid parameters` | Invalid parameters were provided ||
|| `400` | `Incorrect entity type` | An unsupported object type was specified ||
|| `400` | `Entities is not array` | The `ENTITIES` parameter is not an array ||
|| `400` | `Incorrect entities id` | Invalid `ID` of elements were provided ||
|| `403` | `You don't have access to these entities` | No access to the specified elements ||
|| `400` | `Incorrect webform id` | Invalid `ID` of the CRM form ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-statuslist.md)
- [{#T}](./crm-calllist-update.md)