# Get Fields of Product Property Features or Variations catalog.productPropertyFeature.getFields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.productPropertyFeature.getFields` returns a description of the fields for product property features or variations.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Footnote](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyFeature.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyFeature.getFields
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productPropertyFeature.getFields', {});
        console.log(response.getData().result);
    }
    catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyFeature.getFields',
                []
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyFeature.getFields',
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productPropertyFeature.getFields',
        []
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productPropertyFeature": {
        "featureId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
        },
        "id": {
            "isImmutable": false,
            "isReadOnly": true,
            "isRequired": false,
            "type": "integer"
        },
        "isEnabled": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "char"
        },
        "moduleId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
        },
        "propertyId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "integer"
        }
        }
    },
    "time": {
        "start": 1774251804,
        "finish": 1774251804.318509,
        "duration": 0.3185091018676758,
        "processing": 0,
        "date_start": "2026-03-23T10:43:24+01:00",
        "date_finish": "2026-03-23T10:43:24+01:00",
        "operating_reset_at": 1774252404,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response ||
|| **productPropertyFeature**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features), and `value` is an object of type [rest_field_description](../data-types.md#rest_field_description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient rights to view the product catalog ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add.md)
- [{#T}](./catalog-product-property-feature-update.md)
- [{#T}](./catalog-product-property-feature-get.md)
- [{#T}](./catalog-product-property-feature-list.md)
- [{#T}](./catalog-product-property-feature-get-available-features-by-property.md)