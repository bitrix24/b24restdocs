# Get Description of Fields for Inventory Document catalog.document.getFields

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with the "View product catalog" access permission

The method `catalog.document.getFields` returns the description of fields for the inventory document.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.getFields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.document.getFields',
    		{}
    	);

    	const result = response.getData().result;
    	console.log(result);
    }
    catch (error)
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
                'catalog.document.getFields',
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
        echo 'Error getting document fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.getFields',
        {},
        function(result)
        {
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
        'catalog.document.getFields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "document": {
            "commentary": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "createdBy": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "currency": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "char"
            },
            "dateCreate": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateDocument": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateModify": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateStatus": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "docNumber": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "docType": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "char"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "modifiedBy": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "responsibleId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "siteId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "status": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "char"
            },
            "statusBy": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "title": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "total": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            }
        }
    },
    "time": {
        "start": 1761727898,
        "finish": 1761727898.554004,
        "duration": 0.5540039539337158,
        "processing": 0,
        "date_start": "2025-10-29T11:51:38+01:00",
        "date_finish": "2025-10-29T11:51:38+01:00",
        "operating_reset_at": 1761728498,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md#catalog_document) | Root element of the response ||
|| **document**
[`object`](../data-types.md#catalog_document) | Object with the description of fields for the inventory document 
Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. Where `field` is the identifier of the field of the [`catalog_document`](../data-types.md#catalog_document) object, and `value` is an object of type [`rest_field_description`](../data-types.md). ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Insufficient permissions to save the document"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient permissions to save the document | User does not have view permission ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-list.md)
- [{#T}](./catalog-document-add.md)
- [{#T}](./catalog-document-update.md)