# Get data about the digital workplace by id crm.automatedsolution.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

The method returns information about the digital workplace with the identifier `id`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the digital workplace ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":393}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":393,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automatedsolution.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.automatedsolution.get',
    		{
    			'id': 393
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
                'crm.automatedsolution.get',
                [
                    'id' => 393,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting automated solution: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.automatedsolution.get",
        {
            "id": 393
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
        'crm.automatedsolution.get',
        [
            'id' => 393
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
		"automatedSolution": {
			"id": 393,
			"title": "HR",
			"typeIds": [
				4,
				9,
				77,
				157,
				225
			]
		}
	},
	"time": {
		"start": 1715849396.642359,
		"finish": 1715849396.954623,
		"duration": 0.31226396560668945,
		"processing": 0.0068209171295166016,
		"date_start": "2024-05-16T11:49:56+02:00",
		"date_finish": "2024-05-16T11:49:56+02:00",
		"operating_reset_at": 1715849996,
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
|| **automatedSolution**
[`object`](../../data-types.md) | Information about the digital workplace ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

**Object** `automatedSolution`:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the digital workplace ||
|| **title**
[`string`](../../data-types.md) | Name of the digital workplace ||
|| **typeIds**
[`array`](../../data-types.md) | Identifiers of the smart processes linked to the workplace ||
|#

{% note warning %}

`Id` of the digital workplace and `id` of the `customSection` structures from the method [crm.type.get](../universal/user-defined-object-types/crm-type-get.md) do not match due to different storage organization

{% endnote %}

## Error Handling

HTTP status: **400**

```json
{
    "error":"NOT_FOUND",
    "error_description":"Digital workplace with such id not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `NOT_FOUND` | Digital workplace with such `id` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-add.md)
- [{#T}](./crm-automated-solution-update.md)
- [{#T}](./crm-automated-solution-list.md)
- [{#T}](./crm-automated-solution-delete.md)
- [{#T}](./crm-automated-solution-fields.md)