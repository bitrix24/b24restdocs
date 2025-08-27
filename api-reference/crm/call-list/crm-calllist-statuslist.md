# Get the list of call statuses crm.calllist.statuslist

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.calllist.statuslist` returns a list of call statuses.

To create a new status, modify, or delete an existing one, use the methods [crm.status.*](../status/index.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.statuslist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.statuslist
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.calllist.statuslist",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
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
                'crm.calllist.statuslist',
                []
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
        echo 'Error fetching call list status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.calllist.statuslist",
        {},
        function(result) {
            if(result.error())
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
        'crm.calllist.statuslist',
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
    "result": [
        {
            "ID": 61,
            "NAME": "In Progress",
            "SORT": 10,
            "STATUS_ID": "IN_WORK"
        },
        {
            "ID": 63,
            "NAME": "Successful",
            "SORT": 20,
            "STATUS_ID": "SUCCESS"
        },
        {
            "ID": 65,
            "NAME": "Wrong Number",
            "SORT": 30,
            "STATUS_ID": "WRONG_NUMBER"
        },
        {
            "ID": 67,
            "NAME": "Do Not Call Again",
            "SORT": 40,
            "STATUS_ID": "STOP_CALLING"
        }
    ],
    "time": {
        "start": 1752560909.614115,
        "finish": 1752560909.68214,
        "duration": 0.06802511215209961,
        "processing": 0.007961034774780273,
        "date_start": "2025-07-15T09:28:29+02:00",
        "date_finish": "2025-07-15T09:28:29+02:00",
        "operating_reset_at": 1752561509,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of call statuses [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Field Descriptions for result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Status identifier ||
|| **NAME**
[`string`](../../data-types.md) | Name ||
|| **SORT**
[`integer`](../../data-types.md) | Sort order ||
|| **STATUS_ID**
[`string`](../../data-types.md) | Status code, use in the method [crm.calllist.items.get](./crm-calllist-items-get.md) ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-update.md)