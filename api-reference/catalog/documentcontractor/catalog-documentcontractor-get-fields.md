# Get Description of Fields for Binding Vendor to Inventory Document catalog.documentcontractor.getFields  

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the following permissions:
> — "View" on the document type "Incoming",
> — "View Inventory Management section"
> — "View Product Catalog"  

The method `catalog.documentcontractor.getFields` returns the description of fields for binding a vendor, contact, or company to an inventory document.

## Method Parameters  

No parameters.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %} 

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.getFields
    ```

- JS

    ```js  
    try
    {
        const response = await $b24.callMethod(
            'catalog.documentcontractor.getFields',
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
                'catalog.documentcontractor.getFields',
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
        echo 'Error getting contractor binding fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.getFields',
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
        'catalog.documentcontractor.getFields',
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
        "documentContractor": {
            "documentId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "entityId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "entityTypeId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            }
        }
    },
    "time": {
        "start": 1766410081,
        "finish": 1766410081.636695,
        "duration": 0.6366949081420898,
        "processing": 0,
        "date_start": "2025-12-22T16:28:01+01:00",
        "date_finish": "2025-12-22T16:28:01+01:00",
        "operating_reset_at": 1766410681,
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
|| **documentContractor**
[`object`](../data-types.md#catalog_documentContractor) | Object describing the fields for binding a vendor to an inventory document.
Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. Where `field` is the identifier of the field of the [`catalog_documentContractor`](../data-types.md#catalog_documentContractor) object, and `value` is an object of type [`rest_field_description`](../data-types.md) ||  
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling 

{% include notitle [error handling](../../../_includes/error-info.md) %}  

HTTP Code: **400**

```json  
{
    "error": "0",
    "error_description": "Access denied"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access denied | Insufficient permissions to view the document or bindings ||  
|| `0` | Contractors should be provided by CRM | CRM module is not active as a vendor for contractors ||  
|#  

{% include [System Errors](../../../_includes/system-errors.md) %}  

## Continue Learning  

- [{#T}](./catalog-documentcontractor-list.md)  
- [{#T}](./catalog-documentcontractor-add.md)  
- [{#T}](./catalog-documentcontractor-delete.md)  