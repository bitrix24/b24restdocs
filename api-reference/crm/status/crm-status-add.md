# Create CRM Status Element crm.status.add

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with CRM administrator rights

The method `crm.status.add` creates a new element in the specified CRM directory: deal stage, source, company type, and others.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields*** 
[`object`](../../data-types.md) | Object with fields of the new directory element. The list of available fields is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID*** 
[`string`](../../data-types.md) | Type of directory, for example `DEAL_STAGE`, `SOURCE`. You can get the list of types using the method [crm.status.entity.types](./crm-status-entity-types.md) ||
|| **STATUS_ID*** 
[`string`](../../data-types.md) | Status value code. The code must be unique within the directory. Length and character restrictions depend on the type of directory ||
|| **NAME*** 
[`string`](../../data-types.md) | Name ||
|| **SORT** 
[`integer`](../../data-types.md) | Sorting. Default is 10 ||
|| **COLOR** 
[`string`](../../data-types.md) | Hex color code, for example `#39A8EF`. Use for status stages ||
|| **SEMANTICS** 
[`string`](../../data-types.md) | Group of stages:
- `"S"` — "Success", 
- `"F"` — "Failure", 
- `""` — "In Progress".

Use for status stages. Default is "In Progress" group ||
|#

### Field Features

**Restrictions for `STATUS_ID`**

Follow the length and character restrictions for different types of directories:

- **STATUS** lead stages. Max length: 21, can contain only Latin letters, numbers, hyphens, and underscores.
- **QUOTE_STATUS** estimate stages. Max length: 22, can contain only Latin letters, numbers, hyphens, and underscores.
- **DEAL_STAGE** deal stages. Max length: 22, can contain only Latin letters, numbers, hyphens, and underscores.
- **DEAL_STAGE_xx** deal stages in non-default funnels. xx — funnel identifier. Max length depends on the funnel identifier. Can contain only Latin letters, numbers, hyphens, and underscores.
- For other `ENTITY_ID`, the maximum length of `STATUS_ID` is 50 characters and can contain any characters.

If a stage is added for a custom deal funnel, the status identifier will automatically have the funnel prefix added. This is necessary to identify the funnel by the stage identifier.

For example, the value `DECISION` for a deal funnel with identifier `1` will be saved as `C1:DECISION`. The system will automatically add the prefix `C1:` corresponding to the deal funnel identifier. 
If a value is passed to the field with the prefix `C1:DECISION`, it will be saved as `C1:DECISION`, and no additional prefix will be added.

For SPAs with funnels, a similar logic for forming `STATUS_ID` from the value and prefix applies. The funnel prefix can be obtained using the method [crm.status.entity.types](./crm-status-entity-types.md).

**Restrictions for `SORT`**

For the correct operation of status stages, the sorting must follow this order: 
1. Stages of the "In Progress" group
2. Stage of the "Success" group
3. Stages of the "Failure" group

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"fields":{"ENTITY_ID":"DEAL_STAGE_1","STATUS_ID":"DECISION","NAME":"Decision Making","SORT":70,"COLOR":"#FFA900"}}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":"DEAL_STAGE_1","STATUS_ID":"DECISION","NAME":"Decision Making","SORT":70,"COLOR":"#FFA900"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.status.add',
    		{
    			fields: {
    				ENTITY_ID: 'DEAL_STAGE_1',
    				STATUS_ID: 'DECISION',
    				NAME: 'Decision Making',
    				SORT: 70,
    				COLOR: '#FFA900'
    			}
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
                'crm.status.add',
                [
                    'fields' => [
                        'ENTITY_ID' => 'DEAL_STAGE_1',
                        'STATUS_ID' => 'DECISION',
                        'NAME'     => 'Decision Making',
                        'SORT'     => 70,
                        'COLOR'    => '#FFA900',
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
        echo 'Error adding status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.status.add",
        {
            fields: {
                ENTITY_ID: "DEAL_STAGE_1",
                STATUS_ID: "DECISION",
                NAME: "Decision Making",
                SORT: 70,
                COLOR: "#FFA900"
            }
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
        'crm.status.add',
        [
            'fields' => [
                'ENTITY_ID' => 'DEAL_STAGE_1',
                'STATUS_ID' => 'DECISION',
                'NAME' => 'Decision Making',
                'SORT' => 70,
                'COLOR' => '#FFA900'
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
    "result": 773,
    "time": {
        "start": 1752215174.862923,
        "finish": 1752215174.916697,
        "duration": 0.053774118423461914,
        "processing": 0.014070987701416016,
        "date_start": "2025-07-11T09:26:14+02:00",
        "date_finish": "2025-07-11T09:26:14+02:00",
        "operating_reset_at": 1752215774,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`integer`](../../data-types.md) | Identifier of the created directory element ||
|| **time** 
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Invalid parameters.",
    "error_description": "Invalid parameters were provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `Access denied.` | No rights to perform the operation ||
|| `400`     | `Invalid parameters.` | Invalid parameters were provided ||
|| `400`     | `Specified entity type is not supported.` | An unsupported directory type was specified ||
|| `400`     | `The field ENTITY_ID is required.` | `ENTITY_ID` is not specified ||
|| `400`     | `The field STATUS_ID is required.` | `STATUS_ID` is not specified ||
|| `400`     | `Duplicate STATUS_ID.` | Such `STATUS_ID` already exists ||
|| `400`     | `Error on creating status.` | Error creating the element ||
|| `400`     | ` ` | Cannot create an intermediate stage after success ||
|| `400`     | ` ` | The required field "Title" is not filled ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}


## Continue Learning

- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-get.md)
- [{#T}](./crm-status-update.md)
- [{#T}](./crm-status-delete.md)
- [{#T}](./crm-status-fields.md)
- [{#T}](./crm-status-entity-types.md) 
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)