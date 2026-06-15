# Create a Price Rounding Rule catalog.roundingRule.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a price rounding rule.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Field values for creating a new price rounding rule ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId***
[`catalog_price_type.id`](../data-types.md#catalog_price_type) | Price type ||
|| **price***
[`double`](../../data-types.md) | Minimum price for rounding ||
|| **roundType***
[`integer`](../../data-types.md) | Rounding type. Possible values:
- `1` — mathematical rounding
- `2` — rounding up (in favor of the store)
- `4` — rounding down (in favor of the customer)
||
|| **roundPrecision***
[`double`](../../data-types.md) | Rounding precision ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":14,"price":1000,"roundType":4,"roundPrecision":100}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingRule.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":14,"price":1000,"roundType":4,"roundPrecision":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.roundingRule.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RoundingRuleAddResult = {
      roundingRule: {
        catalogGroupId: number
        createdBy: number
        dateCreate: ISODate | null
        dateModify: ISODate | null
        id: number
        modifiedBy: number
        price: number
        roundPrecision: number
        roundType: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<RoundingRuleAddResult>({
        method: 'catalog.roundingRule.add',
        params: {
          fields: {
            catalogGroupId: 14,
            price: 1000,
            roundType: 4,
            roundPrecision: 100,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.roundingRule.id, result.roundingRule.roundType, result.roundingRule.roundPrecision)
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
      async function addRoundingRule() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.roundingRule.add',
            params: {
              fields: {
                catalogGroupId: 14,
                price: 1000,
                roundType: 4,
                roundPrecision: 100,
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
          console.info(result.roundingRule.id, result.roundingRule.roundType, result.roundingRule.roundPrecision)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addRoundingRule)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.roundingRule.add',
                [
                    'fields' => [
                        'catalogGroupId' => 14,
                        'price'          => 1000,
                        'roundType'      => 4,
                        'roundPrecision' => 100
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding rounding rule: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'catalog.roundingRule.add',
            {
                fields: {
                        catalogGroupId: 14,
                price: 1000,
                roundType: 4,
                roundPrecision: 100
                }
            },
    function(result) {
            if (result.error())
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
        'catalog.roundingRule.add',
        [
            'fields' => [
                'catalogGroupId' => 14,
                'price' => 1000,
                'roundType' => 4,
                'roundPrecision' => 100
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
        "roundingRule": {
            "catalogGroupId": 14,
            "createdBy": 1,
            "dateCreate": "2024-09-25T16:40:59+02:00",
            "dateModify": "2024-09-25T16:40:59+02:00",
            "id": 1,
            "modifiedBy": 1,
            "price": 1000,
            "roundPrecision": 100,
            "roundType": 4
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-09-25T16:40:59+02:00",
        "date_finish": "2024-09-25T16:40:59+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **roundingRule**
[`catalog_rounding_rule`](../data-types.md#catalog_rounding_rule) | Object with information about the created price rounding rule ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300020,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `100` | Required parameter `fields` not provided
||
|| `0` | Required fields not set
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-rounding-rule-update.md)
- [{#T}](./catalog-rounding-rule-get.md)
- [{#T}](./catalog-rounding-rule-list.md)
- [{#T}](./catalog-rounding-rule-delete.md)
- [{#T}](./catalog-rounding-rule-get-fields.md)