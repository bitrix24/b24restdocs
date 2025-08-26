# Add Logo crm.timeline.logo.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `administrator`

This method adds a new logo.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../../../data-types.md) | Logo code (for example, `info`) ||
|| **fileContent***
[`string`](../../../../data-types.md) | Base64 encoded content of the logo file.

File requirements:

- Type — png
- Size — 60x60 pixels
- Background — transparent 

||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":10,"ENTITY_TYPE":"deal","COMMENT":"New comment was added","AUTHOR_ID":5,"FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":10,"ENTITY_TYPE":"deal","COMMENT":"New comment was added","AUTHOR_ID":5,"FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.timeline.logo.add',
    		{
    			code: 'info',
    			fileContent: 'iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU',
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
                'crm.timeline.logo.add',
                [
                    'code'        => 'info',
                    'fileContent' => 'iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU',
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
        echo 'Error adding timeline logo: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.logo.add",
        {
            code: "info",
            fileContent: "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU",
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
        'crm.timeline.comment.add',
        [
            'fields' => [
                'ENTITY_ID' => 10,
                'ENTITY_TYPE' => 'deal',
                'COMMENT' => 'New comment was added',
                'AUTHOR_ID' => 5,
                'FILES' => [
                    ["1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],
                    ["2.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]
                ]
            ]
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
        "logo": {
            "code": "info",
            "isSystem": false,
            "fileUri": "/upload/crm/286/a1k092hygdrzpiz6a01enuymqoo5qzym/ou0akdwnbxalzk9hgfme39nbvtozblew"
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
[`object`](../../../../data-types.md) | Root element of the response.

The `result` field contains the [logo](#logo) object ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### Logo Object {#logo}

#|
|| **Field**
`type`  | **Description** ||
||**code** | Logo code ||
||**isSystem** | Flag field. Can have the value:
- `true` — if the logo is standard (provided with the product)
- `false` — if the logo was added by the user 
||
||**fileUri** | Path to the file.

If the logo was added by the user, the field contains the path to the logo image file ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Access denied ||
|| `INVALID_ARG_VALUE` | Invalid `fileContent` parameter specified ||
|| `FILE_SAVE_ERROR` | Unable to save the provided logo file ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-logo-get.md)
- [{#T}](./crm-timeline-logo-list.md)
- [{#T}](./crm-timeline-logo-delete.md)