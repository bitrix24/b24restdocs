# Set Section Settings for Property catalog.productPropertySection.set

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

The method `catalog.productPropertySection.set` sets the section settings for a product property or variation.

If there are no records for the section settings of the property, the method creates one. If the record already exists, the method updates it.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the product property or variation.

Property identifiers can be obtained using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Fields for the section settings of the property [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **smartFilter**
[`char`](../../data-types.md) | Whether to show the property in the smart filter. Possible values:
- `Y` — yes
- `N` — no ||
|| **displayType**
[`char`](../../data-types.md) | Type of the property in the smart filter. Possible values:
- `F` — checkboxes
- `K` — radio buttons
- `P` — dropdown list ||
|| **displayExpanded**
[`char`](../../data-types.md) | Whether to show the property expanded in the filter. Possible values:
- `Y` — yes
- `N` — no ||
|| **filterHint**
[`string`](../../data-types.md) | Hint in the smart filter for visitors ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"propertyId":901,"fields":{"smartFilter":"Y","displayType":"F","displayExpanded":"N","filterHint":"Filter hint"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertySection.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"propertyId":901,"fields":{"smartFilter":"Y","displayType":"F","displayExpanded":"N","filterHint":"Filter hint"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertySection.set
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productPropertySection.set', {
            propertyId: 901,
            fields: {
                smartFilter: 'Y',
                displayType: 'F',
                displayExpanded: 'N',
                filterHint: 'Filter hint'
            }
        });

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
                'catalog.productPropertySection.set',
                [
                    'propertyId' => 901,
                    'fields' => [
                        'smartFilter' => 'Y',
                        'displayType' => 'F',
                        'displayExpanded' => 'N',
                        'filterHint' => 'Filter hint',
                    ]
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
        'catalog.productPropertySection.set',
        {
            propertyId: 901,
            fields: {
                smartFilter: 'Y',
                displayType: 'F',
                displayExpanded: 'N',
                filterHint: 'Filter hint'
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
        'catalog.productPropertySection.set',
        [
            'propertyId' => 901,
            'fields' => [
                'smartFilter' => 'Y',
                'displayType' => 'F',
                'displayExpanded' => 'N',
                'filterHint' => 'Filter hint',
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
        "productPropertySection": {
        "displayExpanded": "N",
        "displayType": "F",
        "filterHint": "Filter hint",
        "iblockId": "25",
        "propertyId": "901",
        "sectionId": "0",
        "smartFilter": "Y"
        }
    },
    "time": {
        "start": 1774265022,
        "finish": 1774265022.693577,
        "duration": 0.6935770511627197,
        "processing": 0,
        "date_start": "2026-03-23T14:23:42+02:00",
        "date_finish": "2026-03-23T14:23:42+02:00",
        "operating_reset_at": 1774265622,
        "operating": 0.10450911521911621
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response ||
|| **productPropertySection**
[`catalog_product_property_section`](../data-types.md#catalog_product_property_section) | Object of section settings for the property ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "productPropertySection does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient permissions to modify the section settings of the property ||
|| `0` | productPropertySection does not exist. | The property with the provided `propertyId` was not found or does not belong to the product catalog ||
|| `100` | Could not find value for parameter {fields} | The required parameter `fields` was not provided ||
|| `0` | Error setting section properties | Internal error while saving the section settings of the property ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-section-get.md)
- [{#T}](./catalog-product-property-section-list.md)