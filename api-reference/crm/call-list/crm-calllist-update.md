# Update Call List Composition crm.calllist.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: an administrator, the creator of the call list, or the user responsible for the call list activity. Read permission for contacts or companies is also required, depending on the type of call list participants

The `crm.calllist.update` method replaces the call list participants and the attached CRM form.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LIST_ID***
[`integer`](../../data-types.md) | Identifier of the call list. You can retrieve it using the [crm.calllist.add](./crm-calllist-add.md) and [crm.calllist.list](./crm-calllist-list.md) methods ||
|| **ENTITY_TYPE***
[`string`](../../data-types.md) | Type of entity:
- `CONTACT` — contact
- `COMPANY` — company ||
|| **ENTITIES***
[`integer[]`](../../data-types.md) | Array of numeric identifiers of contacts or companies that must remain in the call list. You can retrieve them using the [crm.item.list](../universal/crm-item-list.md) method ||
|| **WEBFORM_ID**
[`integer`](../../data-types.md) | `ID` of the CRM form for the call list: during a call, the employee fills in the client's data in it.
The identifier is returned by the [crm.form.list](../form/crm-form-list.md) method ||
|#

### Method Operation Features

The method replaces the call list participants with the `ENTITIES` array from the request. Pass everyone who must remain in the call list:

- to add participant `4` to a call list with participants `[1,2,3]`, pass `[1,2,3,4]`
- to remove participant `2` from the same call list, pass `[1,3]`

The method also overwrites `WEBFORM_ID`: if the parameter is not passed, the form is detached.

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":123,"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.calllist.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":123,"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5,"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.calllist.update
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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
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
|| `400` | `ENTITIES_ERROR` | `Incorrect entities id` | The `ENTITIES` parameter contains an empty array, duplicates, or identifiers that do not exist in CRM ||
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
- [{#T}](./index.md)