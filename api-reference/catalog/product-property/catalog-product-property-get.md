# Get Values of Product or Variation Property Fields catalog.productProperty.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to view the catalog

The method `catalog.productProperty.get` returns the values of the product or variation property fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the property. 

Property identifiers can be obtained using the [catalog.productProperty.list](./catalog-product-property-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":659}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productProperty.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":659,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productProperty.get
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productProperty.get', { id: 659 });
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
                'catalog.productProperty.get',
                [
                    'id' => 659,
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
        'catalog.productProperty.get',
        {
            id: 659
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
        'catalog.productProperty.get',
        ['id' => 659]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productProperty": {
            "active": "Y",
            "code": "s12",
            "colCount": 30,
            "defaultValue": null,
            "fileType": null,
            "filtrable": "N",
            "hint": null,
            "iblockId": 19,
            "id": 659,
            "isRequired": "N",
            "linkIblockId": null,
            "listType": "L",
            "multiple": "N",
            "multipleCnt": null,
            "name": "Category",
            "propertyType": "S",
            "rowCount": 1,
            "searchable": "N",
            "sort": null,
            "timestampX": "2026-03-19T21:23:02+02:00",
            "userType": "directory",
            "userTypeSettings": {
                "group": "N",
                "multiple": "N",
                "size": 1,
                "tableName": "b_hlbd_categories",
                "width": 0
            },
            "withDescription": null,
            "xmlId": null
        }
    },
    "time": {
        "start": 1773950078,
        "finish": 1773950078.362409,
        "duration": 0.3624091148376465,
        "processing": 0,
        "date_start": "2026-03-19T22:54:38+02:00",
        "date_finish": "2026-03-19T22:54:38+02:00",
        "operating_reset_at": 1773950678,
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
|| **productProperty**
[`catalog_product_property`](../data-types.md#catalog_product_property) | Object containing information about the product or variation property ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "productProperty does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Access Denied | Insufficient rights to view the catalog ||
|| `400` | Empty value | productProperty does not exist | Property with the specified `id` not found ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-add.md)
- [{#T}](./catalog-product-property-update.md)
- [{#T}](./catalog-product-property-list.md)
- [{#T}](./catalog-product-property-delete.md)
- [{#T}](./catalog-product-property-get-fields.md)