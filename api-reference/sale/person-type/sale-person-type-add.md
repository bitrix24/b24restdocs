# Add payer type sale.persontype.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a new payer type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parameter-fields)) for creating a new payer type in the form of a structure:

```js
fields: {
    name: 'value',
    code: 'value',
    sort: 'value',
    active: 'value',
    xmlId: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the payer type ||
|| **code**
[`string`](../../data-types.md) | Code of the payer type. Must be unique ||
|| **sort**
[`string`](../../data-types.md) | Sorting. Default value is `150` ||
|| **active**
[`string`](../../data-types.md) | Active flag. Can take values `Y` / `N`. Default is set to `Y` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier.

Can be used to synchronize the current payer type with a similar position in an external system
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
    -d '{"fields":{"name":"Individual","sort":"100","active":"Y","code":"MY_CRM_COMPANY","xmlId":"myXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.persontype.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Individual","sort":"100","active":"Y","code":"MY_CRM_COMPANY","xmlId":"myXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.persontype.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.persontype.add', 
    		{
    			fields: {
    				name: 'Individual',
    				sort: '100',
    				active: 'Y',
    				code: 'MY_CRM_COMPANY',
    				xmlId: 'myXmlId'
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
                'sale.persontype.add',
                [
                    'fields' => [
                        'name'   => 'Individual',
                        'sort'   => '100',
                        'active' => 'Y',
                        'code'   => 'MY_CRM_COMPANY',
                        'xmlId'  => 'myXmlId',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding person type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.persontype.add', 
        {
            fields: {
                name: 'Individual',
                sort: '100',
                active: 'Y',
                code: 'MY_CRM_COMPANY',
                xmlId: 'myXmlId'
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
        'sale.persontype.add',
        [
            'fields' => [
                'name' => 'Individual',
                'sort' => '100',
                'active' => 'Y',
                'code' => 'MY_CRM_COMPANY',
                'xmlId' => 'myXmlId'
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
        "personType": {
            "active": "Y",
            "code": "MY_CRM_COMPANY",
            "id": 68,
            "name": "Individual",
            "sort": "100",
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1712325812.35051,
        "finish": 1712325812.58676,
        "duration": 0.236255884170532,
        "processing": 0.011207103729248,
        "date_start": "2024-04-05T16:03:32+02:00",
        "date_finish": "2024-04-05T16:03:32+02:00",
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
|| **personType**
[`sale_person_type`](../data-types.md) | Object with information about the added payer type ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200750000005,
    "error_description": "person type code exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200750000005` | Insufficient permissions to execute the method ||
|| `200750000001`
`200750000006 ` | Failed to create a new payer type ||
|| `200040300020` | No access to edit ||
|| `100` | Required parameter `fields` not provided ||
|| `0` | Required fields not set ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}