# Create a New Deal from the Template crm.deal.recurring.expose

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" and "add" deal permissions

The method `crm.deal.recurring.expose` creates a new deal based on the template for a recurring deal.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the recurring deal template setting ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.expose
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.expose
    ```

- JS

    ```js
    try
    {
    	const id = 15;
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.expose',
    		{
    			id: id,
    		}
    	);
    
    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    $id = 15;

    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.recurring.expose',
                [
                    'id' => $id,
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
        echo 'Error exposing recurring deal: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = 15;
    BX24.callMethod(
        'crm.deal.recurring.expose',
        {
            id: id
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.recurring.expose',
        [
            'id' => 15
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
        "DEAL_ID": 7589
    },
    "time": {
        "start": 1772692903,
        "finish": 1772692904.118889,
        "duration": 1.1188890933990479,
        "processing": 1,
        "date_start": "2026-03-05T09:41:43+01:00",
        "date_finish": "2026-03-05T09:41:44+01:00",
        "operating_reset_at": 1772693503,
        "operating": 0.7653989791870117
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains data about the created deal ||
|| **result.DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the created deal ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "id is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `id is not defined or invalid` | The parameter `id` has been passed with an empty or invalid value ||
|| `Recurring is not allowed` | Recurring deals are not available in Bitrix24 ||
|| `Recurring deal is not found` | The template for the recurring deal was not found ||
|| `Access denied` | Insufficient permissions to read the original deal or create a new deal ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-add.md)
- [{#T}](./crm-deal-recurring-fields.md)
- [{#T}](./crm-deal-recurring-get.md)
- [{#T}](./events/on-crm-deal-recurring-expose.md)