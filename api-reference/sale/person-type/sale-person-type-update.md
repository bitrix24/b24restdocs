# Change the payer type sale.persontype.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method modifies the fields of the payer type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type`| **Description** ||
|| **id***
[`sale_person_type.id`](../data-types.md) | The ID of the payer type ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parameter-fields)) for updating the payer type field in the form of a structure:

```js
fields: {
    name: 'value',
    code: 'value',
    sort: 'value',
    active: 'value',
    xmlId: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | The name of the payer type ||
|| **code**
[`string`](../../data-types.md) | The code of the payer type. Must be unique ||
|| **sort**
[`string`](../../data-types.md) | Sorting. Default value is `150` ||
|| **active**
[`string`](../../data-types.md) | Activity flag. Can take values `Y` / `N`. Default is set to `Y` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier.

Can be used to synchronize the current payer type with a similar position in an external system
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":12,"fields":{"name":"Legal entity"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.persontype.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":12,"fields":{"name":"Legal entity"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.persontype.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PersonTypeUpdateResult = {
      personType: {
        active: string
        code: string
        id: number
        name: string
        sort: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PersonTypeUpdateResult>({
        method: 'sale.persontype.update',
        params: {
          id: 12,
          fields: {
            name: 'Legal entity',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.personType.id, result.personType.name, result.personType.active)
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
      async function updatePersonType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.persontype.update',
            params: {
              id: 12,
              fields: {
                name: 'Legal entity',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.personType.id, result.personType.name, result.personType.active)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePersonType)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.persontype.update',
                [
                    'id' => 12,
                    'fields' => [
                        'name' => 'Legal entity'
                    ]
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
        echo 'Error updating person type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.persontype.update', 
        {
            id: 12,
            fields: {
                name: 'Legal entity'
            }
        }, 
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.persontype.update',
        [
            'id' => 12,
            'fields' => [
                'name' => 'Legal entity'
            ]
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
    "result": {
        "personType": {
        "active": "Y",
        "code": "MY_CRM_COMPANY",
        "id": 68,
        "name": "Legal entity",
        "sort": "100",
        "xmlId": "1234"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-04-05T16:24:46+02:00",
        "date_finish": "2024-04-05T16:24:46+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **personType**
[`sale_person_type`](../data-types.md) | Object with information about the updated payer type ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200740400001` | The payer type with the specified `id` does not exist ||
|| `200750000007`
`200750000002` | Error updating the type ||
|| `200040300020` | No access to edit ||
|| `100` | Parameter `id` not specified ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields of the `fields` structure not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-person-type-add.md)
- [{#T}](./sale-person-type-get.md)
- [{#T}](./sale-person-type-list.md)
- [{#T}](./sale-person-type-delete.md)
- [{#T}](./sale-person-type-get-fields.md)
