# Get Fields and Settings of Order Property for a Specific Property Type sale.property.getfieldsbytype

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the available fields of order properties by property type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../data-types.md) | Order property type
Possible values:
- `STRING`
- `Y/N`
- `NUMBER`
- `ENUM`
- `FILE`
- `DATE`
- `LOCATION`
- `ADDRESS`
 ||

|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"NUMBER"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.property.getfieldsbytype
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"NUMBER","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.property.getfieldsbytype
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.property.getfieldsbytype", {
    			"type": "NUMBER",
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'sale.property.getfieldsbytype',
                [
                    'type' => 'NUMBER',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting fields by type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.property.getfieldsbytype", {
            "type": "NUMBER",
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.property.getfieldsbytype',
        [
            'type' => 'NUMBER'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result": {
      "property": {
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
         "defaultValue": {
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
         "id": {
            "isImmutable": false,
            "isReadOnly": true,
            "isRequired": false,
            "type": "integer"
         },
         "isFiltered": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
         },
         "multiple": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
         },
         "name": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
         },
         "personTypeId": {
            "isImmutable": true,
            "isReadOnly": false,
            "isRequired": true,
            "type": "integer"
         },
         "propsGroupId": {
            "isImmutable": true,
            "isReadOnly": false,
            "isRequired": true,
            "type": "integer"
         },
         "required": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
         },
         "settings": {
            "fields": {
               "max": {
                  "isImmutable": false,
                  "isReadOnly": false,
                  "isRequired": false,
                  "type": "integer"
               },
               "min": {
                  "isImmutable": false,
                  "isReadOnly": false,
                  "isRequired": false,
                  "type": "integer"
               },
               "step": {
                  "isImmutable": false,
                  "isReadOnly": false,
                  "isRequired": false,
                  "type": "integer"
               }
            },
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "datatype"
         },
         "sort": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "integer"
         },
         "type": {
            "isImmutable": true,
            "isReadOnly": false,
            "isRequired": true,
            "type": "string"
         },
         "userProps": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
         },
         "util": {
            "isImmutable": false,
            "isReadOnly": false,
            "isRequired": false,
            "type": "char"
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
      "start": 1712325081.703631,
      "finish": 1712325082.067712,
      "duration": 0.36408114433288574,
      "processing": 0.023890972137451172,
      "date_start": "2024-04-05T16:51:21+02:00",
      "date_finish": "2024-04-05T16:51:22+02:00"
   }
}
```

## Error Handling

HTTP status: **400**

```json
{
   "error": 0,
   "error_description": "error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read available fields of order properties ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-add.md)
- [{#T}](./sale-property-update.md)
- [{#T}](./sale-property-get.md)
- [{#T}](./sale-property-list.md)
- [{#T}](./sale-property-delete.md)