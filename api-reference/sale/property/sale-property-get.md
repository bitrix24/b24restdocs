# Get Order Property sale.property.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the order property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property.id`](../data-types.md) | Identifier of the order property ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":22}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.property.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":22,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.property.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SalePropertyGetResult = {
      property: {
        active: string
        code: string
        defaultValue: string
        description: string
        id: number
        inputFieldLocation: string
        isAddress: string
        isAddressFrom: string
        isAddressTo: string
        isEmail: string
        isFiltered: string
        isLocation: string
        isLocation4tax: string
        isPayer: string
        isPhone: string
        isProfileName: string
        isZip: string
        multiple: string
        name: string
        personTypeId: number
        propsGroupId: number
        required: string
        settings: unknown[]
        sort: number
        type: string
        userProps: string
        util: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<SalePropertyGetResult>({
        method: 'sale.property.get',
        params: {
          id: 22,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.property.id, result.property.name, result.property.type)
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
      async function getSaleProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.property.get',
            params: {
              id: 22,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.property.id, result.property.name, result.property.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSaleProperty)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.property.get',
                [
                    'id' => 22
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting sale property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.property.get", {
            "id": 22
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.property.get',
        [
            'id' => 22
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
    "result": {
        "property": {
            "active": "Y",
            "code": "PHONE",
            "defaultValue": "",
            "description": "",
            "id": 22,
            "inputFieldLocation": "0",
            "isAddress": "N",
            "isAddressFrom": "N",
            "isAddressTo": "N",
            "isEmail": "N",
            "isFiltered": "N",
            "isLocation": "N",
            "isLocation4tax": "N",
            "isPayer": "N",
            "isPhone": "Y",
            "isProfileName": "N",
            "isZip": "N",
            "multiple": "N",
            "name": "Phone",
            "personTypeId": 3,
            "propsGroupId": 5,
            "required": "Y",
            "settings": [],
            "sort": 120,
            "type": "STRING",
            "userProps": "Y",
            "util": "N",
            "xmlId": ""
        }
    },
    "time": {
        "start": 1712818820.026505,
        "finish": 1712818820.232912,
        "duration": 0.2064070701599121,
        "processing": 0.018364906311035156,
        "date_start": "2024-04-11T10:00:20+02:00",
        "date_finish": "2024-04-11T10:00:20+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **property**
[`sale_order_property`](../data-types.md) | Information about the order property ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":200840400001,
   "error_description":"property does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200840400001` | Order property not found ||
|| `200040300010` | Insufficient permissions to read the order property ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-add.md)
- [{#T}](./sale-property-update.md)
- [{#T}](./sale-property-list.md)
- [{#T}](./sale-property-delete.md)
- [{#T}](./sale-property-get-fields-by-type.md)
