# Get leads, contacts, and companies with matching data crm.duplicate.findbycomm

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: user with read access permission to CRM entities

The method `crm.duplicate.findbycomm` returns the identifiers of leads, contacts, and companies that contain phone numbers or email addresses from a specified list. The search does not consider the phone extension.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../data-types.md) | Type of communication. Possible values:
- `EMAIL` — email address
- `PHONE` — phone ||
|| **values***
[`string[]`](../../data-types.md) | Array of emails or phone numbers.  

Maximum number of values — 20 ||
|| **entity_type**
[`string`](../../data-types.md) | Type of object. Possible values:
- `LEAD` — lead
- `CONTACT` — contact
- `COMPANY` — company

If not specified — the search is performed across all three types ||
|#

### Method Operation Features

If 20 or more duplicates are found for one object, the other types are not returned. For example, if `entity_type` is not specified and duplicates are expected across all three objects, but there are 20 or more duplicates in leads, contacts and companies will not be returned. If there are 20 or more duplicates in contacts, we will receive duplicates for leads and contacts, while the company will be absent from the selection.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"entity_type":"CONTACT","type":"PHONE","values":["1234567","11223355"]}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.duplicate.findbycomm
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**","entity_type":"CONTACT","type":"PHONE","values":["1234567","11223355"]}' \
         https://**put_your_bitrix24_address**/rest/crm.duplicate.findbycomm
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.duplicate.findbycomm',
    		{
    			entity_type: 'CONTACT',
    			type: 'PHONE',
    			values: ['1234567', '11223355']
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error()) {
    		console.error(result.error());
    	} else {
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
                'crm.duplicate.findbycomm',
                [
                    'entity_type' => 'CONTACT',
                    'type'        => 'PHONE',
                    'values'      => ['1234567', '11223355'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Duplicate data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error finding duplicates by communication: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.duplicate.findbycomm",
        {
            entity_type: "CONTACT",
            type: "PHONE",
            values: ["1234567", "11223355"]
        },
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
        'crm.duplicate.findbycomm',
        [
            'entity_type' => 'CONTACT',
            'type' => 'PHONE',
            'values' => ['1234567', '11223355']
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
        "CONTACT": [275, 2297]
    },
    "time": {
        "start": 1750684060.672785,
        "finish": 1750684060.724903,
        "duration": 0.05211806297302246,
        "processing": 0.018191099166870117,
        "date_start": "2025-06-23T16:07:40+02:00",
        "date_finish": "2025-06-23T16:07:40+02:00",
        "operating_reset_at": 1750684660,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **LEAD**
[`integer[]`](../../data-types.md) | Array of identifiers of found leads ||
|| **CONTACT**
[`integer[]`](../../data-types.md) | Array of identifiers of found contacts ||
|| **COMPANY**
[`integer[]`](../../data-types.md) | Array of identifiers of found companies ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Communication type is not defined",
    "error_description": "Parameter 'type' is required."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `403` | `Access denied` | User does not have permission to read CRM entities ||
|| `400` | `Communication type is not defined` | Required parameter `type` is not specified ||
|| `400` | `Communication type '{type}' is not supported in current context` | An unsupported communication type was specified ||
|| `400` | `Communication values is not defined` | Required parameter `values` is not specified ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-entity-merge-batch.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)