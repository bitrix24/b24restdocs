# Add Contact Binding to Lead crm.lead.contact.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method adds a contact binding to the specified lead.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the lead to which the contact should be added. The lead identifier can be obtained using the [get lead list method](../crm-lead-list.md)  ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a contact to the lead in the form of a structure:

```js
fields:
    {
        "CONTACT_ID": "value",
        "SORT": "value",
        "IS_PRIMARY": "value"
    }
```
 ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`integer`](../../../data-types.md) | The identifier of the contact ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Defaults to `10`  ||
|| **IS_PRIMARY**
[`string`](../../../data-types.md) | Primary contact flag

- `Y` — yes
- `N` — no

Defaults to `N` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"CONTACT_ID":1010,"SORT":10,"IS_PRIMARY":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.contact.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"CONTACT_ID":1010,"SORT":10,"IS_PRIMARY":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.contact.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.lead.contact.add',
    		{
    			id: 1,
    			fields: {
    				'CONTACT_ID': 1010,
    				'SORT': 10,
    				'IS_PRIMARY': 'Y'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.lead.contact.add',
                [
                    'id' => 1,
                    'fields' => [
                        'CONTACT_ID' => 1010,
                        'SORT' => 10,
                        'IS_PRIMARY' => 'Y',
                    ],
                ]
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
        echo 'Error adding lead contact: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
            "crm.lead.contact.add",
            {
                id: 1,
                fields:
                {
                    "CONTACT_ID": 1010,
                    "SORT": 10,
                    "IS_PRIMARY": "Y"
                }
            },
            result => {
                if (result.error())
                    console.error(result.error());
                else
                    console.dir(result.data());
            }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.contact.add',
        [
            'id' => 1,
            'fields' =>
            [
                'CONTACT_ID' => 1010,
                'SORT' => 10,
                'IS_PRIMARY' => 'Y'
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
    "result": true,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The result of the operation ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `NOT_FOUND` | Element not found ||
|| ` ` | Required fields not provided ||
|| ` ` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-contact-delete.md)
- [{#T}](./crm-lead-contact-items-get.md)
- [{#T}](./crm-lead-contact-items-set.md)
- [{#T}](./crm-lead-contact-items-delete.md)
- [{#T}](./crm-lead-contact-fields.md)