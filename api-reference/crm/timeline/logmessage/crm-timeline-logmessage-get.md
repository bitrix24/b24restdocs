# Get Information About the Log Entry crm.timeline.logmessage.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `user with read access permission to the CRM entity containing the entry`

This method retrieves information about a timeline log entry.

{% note info "" %}

It is important to note that the method can only retrieve data about entries that were previously added using [`crm.timeline.logmessage.add`](./crm-timeline-logmessage-add.md). System entries cannot be retrieved using `crm.timeline.logmessage.get`.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Integer identifier of the timeline entry (for example, `1`).

Identifiers can be obtained using the method [`crm.timeline.logmessage.list`](./crm-timeline-logmessage-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.logmessage.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.logmessage.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.timeline.logmessage.get',
    		{
    			id: 1,
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
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
                'crm.timeline.logmessage.get',
                [
                    'id' => 1,
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
        echo 'Error getting log message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.logmessage.get",
        {
            id: 1,
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
        'crm.timeline.logmessage.get',
        [
            'id' => 1
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
        "logMessage": {
            "id": 1,
            "created": "2024-04-03T10:26:32+02:00",
            "authorId": 1,
            "title": "Test title",
            "text": "Test note",
            "iconCode": "info"
        }
    },
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response.

The `result` field contains the [logMessage](./crm-timeline-logmessage-add.md#logMessage) object ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Timeline logmessage not found for id 1"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `NOT_FOUND` | The timeline entry with the specified `id` does not exist ||
|| `100` | Required fields were not provided ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-logmessage-add.md)
- [{#T}](./crm-timeline-logmessage-list.md)
- [{#T}](./crm-timeline-logmessage-delete.md)