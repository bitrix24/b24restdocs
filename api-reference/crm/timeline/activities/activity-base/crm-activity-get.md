# Get information about the activity by ID crm.activity.get

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.get` returns information about the activity by its ID.

## Method parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | The ID of the activity in the timeline, for example `999` ||
|#

## Code examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
     curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{ "id":999 }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.activity.get',
    		{
    			id: 999,
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
                'crm.activity.get',
                [
                    'id' => 999,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting activity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'crm.activity.get',
        {
            id: 999,
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
        'crm.activity.get',
        [
            'id' => 999
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "999",
        "OWNER_ID": "15",
        "OWNER_TYPE_ID": "3",
        "TYPE_ID": "2",
        "PROVIDER_ID": "VOXIMPLANT_CALL",
        "PROVIDER_TYPE_ID": "CALL",
        "PROVIDER_GROUP_ID": null,
        "ASSOCIATED_ENTITY_ID": "0",
        "SUBJECT": "Outgoing call Andrew Johnson",
        "CREATED": "2020-09-27T13:26:55+02:00",
        "LAST_UPDATED": "2021-03-21T20:28:24+02:00",
        "START_TIME": "2020-09-27T13:25:00+02:00",
        "END_TIME": "2020-09-27T19:25:00+02:00",
        "DEADLINE": "2020-09-27T13:25:00+02:00",
        "COMPLETED": "Y",
        "STATUS": "2",
        "RESPONSIBLE_ID": "505",
        "PRIORITY": "2",
        "NOTIFY_TYPE": "1",
        "NOTIFY_VALUE": "15",
        "DESCRIPTION": "",
        "DESCRIPTION_TYPE": "1",
        "DIRECTION": "2",
        "LOCATION": "",
        "SETTINGS": [],
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "AUTHOR_ID": "505",
        "EDITOR_ID": "505",
        "PROVIDER_PARAMS": [],
        "PROVIDER_DATA": null,
        "RESULT_MARK": "0",
        "RESULT_VALUE": null,
        "RESULT_SUM": null,
        "RESULT_CURRENCY_ID": null,
        "RESULT_STATUS": "0",
        "RESULT_STREAM": "0",
        "RESULT_SOURCE_ID": null,
        "AUTOCOMPLETE_RULE": "0"
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

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response. Values for the `result` field correspond to [fields of the object](./crm-activity-fields.md#all-fields) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** ||
|| `Activity is not found` | The activity with the specified ID was not found for the entity in CRM ||
|| `Access denied` | No rights to edit the entity in CRM ||
|| `Application context required` | Invalid `PROVIDER_ID` parameter for the activity created in the context of the application ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./crm-activity-add.md)
- [{#T}](./crm-activity-update.md)
- [{#T}](./crm-activity-delete.md)
- [{#T}](./crm-activity-list.md)
- [{#T}](./crm-activity-communication-fields.md)
- [{#T}](./crm-activity-fields.md)