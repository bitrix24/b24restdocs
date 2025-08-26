# Get Fields of the Sales Funnel crm.category.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves information about the fields of the sales funnels (directions) of the CRM object.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of CRM objects for which to retrieve information about the funnel fields ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.category.fields",
    		{
    			entityTypeId: 2,
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
                'crm.category.fields',
                [
                    'entityTypeId' => 2,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling crm.category.fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.category.fields",
        {
            entityTypeId: 2,
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.category.fields',
        [
            'entityTypeId' => 2
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
        "fields": {
            "id": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ID",
                "upperName": "ID"
            },
            "name": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "NAME",
                "upperName": "NAME"
            },
            "sort": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "SORT",
                "upperName": "SORT"
            },
            "entityTypeId": {
                "type": "integer",
                "isRequired": true,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ENTITY_TYPE_ID",
                "upperName": "ENTITY_TYPE_ID"
            },
            "isDefault": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "IS_DEFAULT",
                "upperName": "IS_DEFAULT"
            },
            "originId": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ORIGIN_ID",
                "upperName": "ORIGIN_ID"
            },
            "originatorId": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ORIGINATOR_ID",
                "upperName": "ORIGINATOR_ID"
            }
        }
    },
    "time": {
        "start": 1718098629.350263,
        "finish": 1718098629.800741,
        "duration": 0.45047783851623535,
        "processing": 0.02919292449951172,
        "date_start": "2024-06-11T11:37:09+02:00",
        "date_finish": "2024-06-11T11:37:09+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing a list of available fields in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field_N` is the identifier of the [object field](#fields), and `value` is an object of type [crm_rest_field_description](../../data-types.md#crm_rest_field_description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Field Descriptions {#fields}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`][1] | Identifier of the funnel (direction) ||
|| **name**
[`string`][1] | Name of the funnel ||
|| **sort**
[`integer`][1] | Sort index ||
|| **entityTypeId**
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) to which the funnel belongs ||
|| **isDefault**
[`boolean`][1] | Indicates whether the funnel is the default funnel ||
|| **originId**
[`string`][1] | Identifier of the data source.

Exists only in deals ||
|| **originatorId**
[`string`][1] | Identifier of the item in the data source.

Exists only in deals ||
|| **isSystem** 
[`boolean`][1] | Indicates whether the funnel is a system funnel.

Exists only in SPAs ||
|| **code**
[`string`][1] | Alias for system funnels.

Exists only in SPAs ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND", 
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `NOT_FOUND` | SPA not found | Occurs with incorrect values for `entityTypeId` ||
|| `ENTITY_TYPE_NOT_SUPPORTED` |  Entity type `{entityTypeName}` is not supported | Occurs if the CRM entity does not support funnels ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}


## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)

[1]: ../../../data-types.md