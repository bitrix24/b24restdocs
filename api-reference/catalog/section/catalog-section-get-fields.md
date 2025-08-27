# Get Fields of the Catalog Section catalog.section.getFields

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `catalog.section.getFields` returns the available fields of the catalog section.

Without parameters.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.getFields
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/catalog.section.getFields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.section.getFields',
    		{}
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
                'catalog.section.getFields',
                []
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
        echo 'Error getting catalog section fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.section.getFields',
        {},
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
        'catalog.section.getFields',
        []
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
        "active": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
        },
        "code": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "string"
        },
        "description": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "string"
        },
        "descriptionType": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "string"
        },
        "iblockId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "integer"
        },
        "iblockSectionId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "integer"
        },
        "id": {
            "isImmutable": false,
            "isReadOnly": true,
            "isRequired": false,
            "type": "integer"
        },
        "name": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
        },
        "sort": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "integer"
        },
        "xmlId": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "string"
        }
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
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [`catalog_section`](../data-types.md#catalog_section), and `value` is an object of type [`rest_field_description`](../data-types.md) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-section-add.md)
- [{#T}](./catalog-section-update.md)
- [{#T}](./catalog-section-get.md)
- [{#T}](./catalog-section-list.md)
- [{#T}](./catalog-section-delete.md)