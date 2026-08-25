# Update Call List Composition crm.calllist.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for CRM entities

The `crm.calllist.update` method allows you to add or remove participants from an existing call list and update the associated CRM form.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LIST_ID***
[`integer`](../../data-types.md) | Identifier of the call list ||
|| **ENTITY_TYPE***
[`string`](../../data-types.md) | Type of entity: 
- `CONTACT` — contact,
- `COMPANY` — company ||
|| **ENTITIES***
[`array`](../../data-types.md) | Array of `ID`s of contacts or companies, which can be obtained using the [crm.item.list](../universal/crm-item-list.md) method ||
|| **WEBFORM_ID**
[`integer`](../../data-types.md) | `ID` of the CRM form that will be displayed in the call form. 
The `ID` can be found in the list of forms in Bitrix24 https://your-domain.com/crm/webform/ ||
|#

### Method Operation Features

The method overwrites the `ENTITIES` array. To add an element, include both current and new `ID`s in the request:

1. Current IDs: [1,2,3].
2. New IDs: [4].
3. Send: [1,2,3,4].

To remove an element, send only those `ID`s that should remain in the list:

4. Current IDs: [1,2,3].
5. Remove: [2].
6. Send: [1,3].

The method overwrites the `WEBFORM_ID` field. If the `WEBFORM_ID` field is not provided when calling the method, it will be cleared.

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":123,"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":123,"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5,"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.calllist.update',
        params: {
          LIST_ID: 123,
          ENTITY_TYPE: 'CONTACT',
          ENTITIES: [1, 2, 3],
          WEBFORM_ID: 5,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Call list updated successfully:', result)
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
      async function updateCallList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.calllist.update',
            params: {
              LIST_ID: 123,
              ENTITY_TYPE: 'CONTACT',
              ENTITIES: [1, 2, 3],
              WEBFORM_ID: 5,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Call list updated successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCallList)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.calllist.update(
            list_id=123,
            entity_type="CONTACT",
            entities=[1, 2, 3],
            webform_id=5,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.calllist.update',
                [
                    'LIST_ID'     => 123,
                    'ENTITY_TYPE' => 'CONTACT',
                    'ENTITIES'    => [1, 2, 3],
                    'WEBFORM_ID'  => 5,
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
        echo 'Error updating call list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.calllist.update",
        {
            LIST_ID: 123,
            ENTITY_TYPE: "CONTACT",
            ENTITIES: [1,2,3],
            WEBFORM_ID: 5
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
        'crm.calllist.update',
        [
            'LIST_ID' => 123,
            'ENTITY_TYPE' => 'CONTACT',
            'ENTITIES' => [1,2,3],
            'WEBFORM_ID' => 5
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.calllist.update", b24.Params{
    	"LIST_ID":     123,
    	"ENTITY_TYPE": "CONTACT",
    	"ENTITIES":    []int{1, 2, 3},
    	"WEBFORM_ID":  5,
    })
    if err != nil {
    	return fmt.Errorf("crm.calllist.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1752562914.533195,
        "finish": 1752562914.606445,
        "duration": 0.07325005531311035,
        "processing": 0.044027090072631836,
        "date_start": "2025-07-15T10:01:54+02:00",
        "date_finish": "2025-07-15T10:01:54+02:00",
        "operating_reset_at": 1752563514,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ENTITY_TYPE_ERROR",
    "error_description": "EntityType is incorrect"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | `LIST_ID is not found`, `ENTITY_TYPE is not found`, `ENTITIES is not found` | A required parameter was not passed ||
|| `400` | `ENTITY_TYPE_ERROR` | `EntityType is incorrect` | The `ENTITY_TYPE` parameter received a value other than `CONTACT` or `COMPANY` ||
|| `400` | `ENTITIES_ERROR` | `Entities is not array` | The `ENTITIES` parameter received a value that is not an array ||
|| `400` | `ENTITIES_ERROR` | `Incorrect entities id` | The `ENTITIES` parameter contains identifiers that do not exist in CRM ||
|| `400` | `LIST_ID_ERROR` | `Incorrect list id or access denied` | A call list with this identifier does not exist, or there is no access to it ||
|| `400` | `ENTITY_TYPE_ERROR` | `Discrepancy between the type of call participants and incoming type` | The entity type in `ENTITY_TYPE` does not match the type of the call list participants ||
|| `400` | `WEBFORM_ERROR` | `Incorrect webform id` | The `WEBFORM_ID` parameter specifies a CRM form that does not exist ||
|| `403` | `ACCESS_ERROR` | `Access Denied` | The user does not have the permission to read contacts or companies ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-statuslist.md)