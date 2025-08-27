# Add a Section to the Catalog catalog.section.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.section.add` adds a section to the catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a new catalog section ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the information block.

To obtain existing identifiers, use [catalog.catalog.list](../catalog/catalog-catalog-list.md) ||
|| **iblockSectionId**
[`catalog_section.id`](../data-types.md#catalog_section) | Identifier of the parent section.

To obtain existing identifiers, use [catalog.section.list](./catalog-section-list.md).

By default, the top level is selected ||
|| **name***
[`string`](../data-types.md) | Name of the catalog section ||
|| **xmlId**
[`string`](../data-types.md) | External identifier.

Can be used to synchronize the current catalog section with a similar position in an external system ||
|| **code**
[`string`](../data-types.md) | Code of the catalog section. Must be unique ||
|| **sort**
[`integer`](../data-types.md) | Sorting.

Default is 500 ||
|| **active**
[`string`](../data-types.md) | Indicator of the catalog section's activity:
- `Y` — active
- `N` — inactive

Default is `Y` ||
|| **description**
[`string`](../data-types.md) | Description ||
|| **descriptionType**
[`string`](../data-types.md) | Type of description. Available types: `text`, `html` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "name": "Children's Toys",
            "iblockId": 14,
            "iblockSectionId": 13,
            "sort": "100",
            "active": "Y",
            "code": "toys",
            "xmlId": "myXmlId",
            "description": "Products for children - toys",
            "descriptionType": "text"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "name": "Children's Toys",
            "iblockId": 14,
            "iblockSectionId": 13,
            "sort": "100",
            "active": "Y",
            "code": "toys",
            "xmlId": "myXmlId",
            "description": "Products for children - toys",
            "descriptionType": "text"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/catalog.section.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.section.add', 
    		{
    			fields: {
    				name: 'Children\'s Toys',
    				iblockId: 14,
    				iblockSectionId: 13,
    				sort: '100',
    				active: 'Y',
    				code: 'toys',
    				xmlId: 'myXmlId',
    				description: "Products for children - toys",
    				descriptionType: "text"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.section.add',
                [
                    'fields' => [
                        'name'            => 'Children\'s Toys',
                        'iblockId'        => 14,
                        'iblockSectionId' => 13,
                        'sort'            => '100',
                        'active'          => 'Y',
                        'code'            => 'toys',
                        'xmlId'           => 'myXmlId',
                        'description'     => "Products for children - toys",
                        'descriptionType' => "text",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding catalog section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.section.add', 
        {
            fields: {
                name: 'Children\'s Toys',
                iblockId: 14,
                iblockSectionId: 13,
                sort: '100',
                active: 'Y',
                code: 'toys',
                xmlId: 'myXmlId',
                description: "Products for children - toys",
                descriptionType: "text"
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
        'catalog.section.add',
        [
            'fields' => [
                'name' => 'Children\'s Toys',
                'iblockId' => 14,
                'iblockSectionId' => 13,
                'sort' => '100',
                'active' => 'Y',
                'code' => 'toys',
                'xmlId' => 'myXmlId',
                'description' => 'Products for children - toys',
                'descriptionType' => 'text'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "section": {
            "active": "Y",
            "code": "toys",
            "description": "Products for children - toys",
            "descriptionType": "text",
            "iblockId": 14,
            "iblockSectionId": 13,
            "id": 31,
            "name": "Children's Toys",
            "sort": 100,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-05-24T14:08:41+02:00",
        "date_finish": "2024-05-24T14:08:41+02:00",
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
|| **section**
[`catalog_section`](../data-types.md#catalog_section) | Object containing information about the added catalog section ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300040,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300040` | No access to editing ||
|| `200700300000` | Errors while adding, for example, the identifier of the information block of the created section does not match the identifier of the parent section ||
|| `200700300040` | Violation of the uniqueness of the `code` field ||
|| `200700300050` | Information block with the specified `iblockId` does not exist ||
|| `100` | Required parameter `fields` not provided ||
|| `0` | Required fields not set ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-update.md)
- [{#T}](./catalog-section-get.md)
- [{#T}](./catalog-section-list.md)
- [{#T}](./catalog-section-delete.md)
- [{#T}](./catalog-section-get-fields.md)