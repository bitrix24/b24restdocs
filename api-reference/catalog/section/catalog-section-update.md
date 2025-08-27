# Update the catalog section catalog.section.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.section.update` modifies a section of the catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_section.id`](../data-types.md#catalog_section) | Identifier of the catalog section ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the catalog section ||
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

To obtain existing identifiers, use [catalog.section.list](./catalog-section-list.md) 
||
|| **name***
[`string`](../data-types.md) | Name of the catalog section ||
|| **xmlId**
[`string`](../data-types.md) | External identifier.

Can be used to synchronize the current catalog section with a similar position in an external system ||
|| **code**
[`string`](../data-types.md) | Code of the catalog section. Must be unique ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **active**
[`string`](../data-types.md) | Indicator of the catalog section's activity:
- `Y` — active
- `N` — inactive ||
|| **description**
[`string`](../data-types.md) | Description of the catalog section ||
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
        "id": 32,
        "fields": {
            "iblockId": 14,
            "name": "Children's Toys",
            "description": "<H1>Children's Toys</H1> <p>Products for kids</p>",
            "descriptionType": "html"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.update
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 32,
        "fields": {
            "iblockId": 14,
            "name": "Children's Toys",
            "description": "<H1>Children's Toys</H1> <p>Products for kids</p>",
            "descriptionType": "html"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/catalog.section.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.section.update', 
    		{
    			id: 32,
    			fields: {
    				iblockId: 14,
    				name: 'Children\'s Toys',
    				description: "<H1>Children's Toys</H1> <p>Products for kids</p>",
    				descriptionType: "html"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
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
                'catalog.section.update',
                [
                    'id' => 32,
                    'fields' => [
                        'iblockId'        => 14,
                        'name'            => 'Children\'s Toys',
                        'description'     => "<H1>Children's Toys</H1> <p>Products for kids</p>",
                        'descriptionType' => "html",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating catalog section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.section.update', 
        {
            id: 32,
            fields: {
                iblockId: 14,
                name: 'Children\'s Toys',
                description: "<H1>Children's Toys</H1> <p>Products for kids</p>",
                descriptionType: "html"
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
        'catalog.section.update',
        [
            'id' => 32,
            'fields' => [
                'iblockId' => 14,
                'name' => 'Children\'s Toys',
                'description' => '<H1>Children\'s Toys</H1> <p>Products for kids</p>',
                'descriptionType' => 'html'
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
        "section": {
            "active": "Y",
            "code": "toys1",
            "description": "<H1>Children's Toys</H1> <p>Products for kids</p>",
            "descriptionType": "html",
            "iblockId": 14,
            "iblockSectionId": 13,
            "id": 32,
            "name": "Children's Toys",
            "sort": 100,
            "xmlId": "myXmlId"
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
[`object`](../../data-types.md) | Root element of the response ||
|| **section**
[`catalog_section`](../data-types.md#catalog_section) | Object with information about the updated catalog section ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
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
|| `200040300040` | No access to edit ||
|| `200700300010` | Errors during update, for example, the identifier of the information block of the updated section does not match the identifier of the parent section ||
|| `200700300030` | No catalog section exists with such an identifier ||
|| `200700300040` | Violation of the uniqueness of the `code` field ||
|| `200700300050` | No information block exists with the specified `iblockId` ||
|| `100` | Parameter `id` not specified ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields of the `fields` structure not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-section-add.md)
- [{#T}](./catalog-section-get.md)
- [{#T}](./catalog-section-list.md)
- [{#T}](./catalog-section-delete.md)
- [{#T}](./catalog-section-get-fields.md)