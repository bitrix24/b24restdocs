# Get the List of Numerators crm.documentgenerator.numerator.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for document generator templates

The method `crm.documentgenerator.numerator.list` returns a list of numerators.

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Offset for pagination. More details in the article [Features of List Methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.numerator.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.numerator.list
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.numerator.list',
    		{
    			start: 0,
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
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
                'crm.documentgenerator.numerator.list',
                [
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting numerators list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.numerator.list',
        {},
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());

            if (result.more()) {
                result.next();
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.numerator.list',
        [
            'start' => 0,
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
        "numerators": [
            {
                "id": "2",
                "name": "Act (USA)",
                "template": "{NUMBER}",
                "settings": {
                    "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                        "start": 1,
                        "step": 1,
                        "periodicBy": null,
                        "timezone": null,
                        "isDirectNumeration": false
                    }
                }
            },
            {
                "id": "45",
                "name": "REST Numerator (updated)",
                "template": "INV-{NUMBER}",
                "settings": {
                    "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                        "start": 100,
                        "step": 1,
                        "length": 0,
                        "padString": "0",
                        "periodicBy": null,
                        "timezone": null,
                        "isDirectNumeration": false
                    }
                }
            }
        ]
    },
    "total": 19,
    "time": {
        "start": 1773750210,
        "finish": 1773750210.283658,
        "duration": 0.2836580276489258,
        "processing": 0,
        "date_start": "2026-03-17T15:23:30+01:00",
        "date_finish": "2026-03-17T15:23:30+01:00",
        "operating_reset_at": 1773750810,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains an array of [`numerators`](#numerators) ||
|| **total**
[`integer`](../../data-types.md) | Total number of numerators ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Array of numerators {#numerators}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | Name of the numerator ||
|| **template**
[`string`](../../data-types.md) | Number template ||
|| **settings**
[`object`](../../data-types.md) | Saved settings for sequential numbering of type [`settings`](#settings) ||
|#

#### Type settings {#settings}

#| 
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Initial value of the counter ||
|| **step**
[`integer`](../../data-types.md) | Increment step of the counter ||
|| **length**
[`integer`](../../data-types.md) | Minimum length of the number ||
|| **padString**
[`string`](../../data-types.md) | Padding character on the left ||
|| **periodicBy**
[`string`](../../data-types.md) | Period for resetting the counter: `null`, `day`, `month`, or `year` ||
|| **timezone**
[`string`](../../data-types.md) | Time zone identifier for periodic reset. Can be `null` ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | Indicator of direct numbering ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "You do not have permissions to modify templates"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value** ||
|| `Empty value` | `You do not have permissions to modify templates` | Insufficient permissions to modify document generator templates ||
|| `Empty value` | `Module documentgenerator is not installed` | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-numerator-add.md)
- [{#T}](./crm-document-generator-numerator-update.md)
- [{#T}](./crm-document-generator-numerator-get.md)
- [{#T}](./crm-document-generator-numerator-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)