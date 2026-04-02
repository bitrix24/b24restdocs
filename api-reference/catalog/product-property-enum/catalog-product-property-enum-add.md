# Add Value to List Property catalog.productPropertyEnum.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" permission and the right to modify the information block property.

The method `catalog.productPropertyEnum.add` adds a value to a list property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Set of fields for the new list value [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the product property or variation.

Property identifiers can be obtained using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method. ||
|| **value***
[`string`](../../data-types.md) | Value of the list item. ||
|| **xmlId***
[`string`](../../data-types.md) | External identifier of the list value. Must be unique within the property. ||
|| **def**
[`char`](../../data-types.md) | Indicator of the default value. Acceptable values:
- `Y` — default
- `N` — not default. ||
|| **sort**
[`integer`](../../data-types.md) | Sort index. ||
|#

{% note info "" %}

The method only adds values for properties of type `L` (list). If a `propertyId` of a different type is provided, the method will return an error `Only list properties are supported`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"fields":{"propertyId":431,"value":"Medium","xmlId":"M","def":"Y","sort":100}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyEnum.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"fields":{"propertyId":431,"value":"Medium","xmlId":"M","def":"Y","sort":100},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyEnum.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productPropertyEnum.add', {
            fields: {
                propertyId: 431,
                value: 'Medium',
                xmlId: 'M',
                def: 'Y',
                sort: 100,
            }
        });

        console.log(response.getData().result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyEnum.add',
                [
                    'fields' => [
                        'propertyId' => 431,
                        'value' => 'Medium',
                        'xmlId' => 'M',
                        'def' => 'Y',
                        'sort' => 100,
                    ],
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyEnum.add',
        {
            fields: {
                propertyId: 431,
                value: 'Medium',
                xmlId: 'M',
                def: 'Y',
                sort: 100,
            }
        },
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
        'catalog.productPropertyEnum.add',
        [
            'fields' => [
                'propertyId' => 431,
                'value' => 'Medium',
                'xmlId' => 'M',
                'def' => 'Y',
                'sort' => 100,
            ]
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productPropertyEnum": {
        "def": "Y",
        "id": 1739,
        "propertyId": 431,
        "sort": 100,
        "value": "Medium",
        "xmlId": "M"
        }
    },
    "time": {
        "start": 1774279799,
        "finish": 1774279799.330864,
        "duration": 0.33086395263671875,
        "processing": 0,
        "date_start": "2026-03-23T18:29:59+02:00",
        "date_finish": "2026-03-23T18:29:59+02:00",
        "operating_reset_at": 1774280399,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response. ||
|| **productPropertyEnum**
[`catalog_product_property_enum`](../data-types.md#catalog_product_property_enum) | Object of the added list property value. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BX_INVALID_VALUE",
    "error_description": "A record with the External code value equal to ... already exists in the database."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient rights to modify the information block property. ||
|| `0` | productPropertyEnum does not exist. | The property with the provided `propertyId` was not found or does not belong to the trade catalog. ||
|| `0` | Only list properties are supported | A property of a type other than `List` was provided. ||
|| `0` | Required fields: xmlId | The required field `xmlId` was not provided. ||
|| `0` | A value with xmlId '...' already exists. | A value with this `xmlId` already exists within the property. ||
|| `BX_INVALID_VALUE` | A record with the value "External code" equal to "..." already exists in the database. | Localized duplicate error for `xmlId`. ||
|| `0` | Internal error adding enumeration value. Try adding again. | Internal error while adding the list value. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-update.md)
- [{#T}](./catalog-product-property-enum-get.md)
- [{#T}](./catalog-product-property-enum-list.md)
- [{#T}](./catalog-product-property-enum-delete.md)
- [{#T}](./catalog-product-property-enum-get-fields.md)