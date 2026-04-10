# Delete the Recurring Deal Template Setting crm.deal.recurring.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "delete" access permission for deals

The method `crm.deal.recurring.delete` removes an existing setting for a recurring deal template. It is suitable when you need to delete the record of a recurring deal template if the associated deal no longer exists.

To delete a recurring deal template, you can use the method [crm.deal.delete](../crm-deal-delete.md). When using this method, both the deal and the configured recurrence template will be deleted.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the recurring deal template setting.

The identifier can be obtained using the methods [crm.deal.recurring.list](./crm-deal-recurring-list.md) or [crm.deal.recurring.add](./crm-deal-recurring-add.md) ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.recurring.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.recurring.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.recurring.delete',
    		{
    			id: 15,
    		}
    	);
    
    	const result = response.getData().result;
    	console.info('Template setting deleted:', result);
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
                'crm.deal.recurring.delete',
                [
                    'id' => 15,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . var_export($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting recurring deal setting: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.recurring.delete',
        {
            id: 15
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
        'crm.deal.recurring.delete',
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
    "result": true,
    "time": {
        "start": 1772759802,
        "finish": 1772759802.471347,
        "duration": 0.4713470935821533,
        "processing": 0.10933709144592285,
        "date_start": "2026-03-06T04:16:42+01:00",
        "date_finish": "2026-03-06T04:16:42+01:00",
        "operating_reset_at": 1772760402,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` for successful deletion ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Deleting is impossible. Connected recurring deal exists."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `id is not defined or invalid` | The `id` parameter has been passed with an empty or incorrect value ||
|| `Recurring is not allowed` | Recurring deals are not available in Bitrix24 ||
|| `Recurring deal is not found` | The recurring deal template was not found ||
|| `Deleting is impossible. Connected recurring deal exists` | Cannot delete the setting while a related deal template exists ||
|| `Access denied` | Insufficient permissions to delete deals ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-recurring-fields.md)
- [{#T}](./crm-deal-recurring-get.md)
- [{#T}](./crm-deal-recurring-list.md)
- [{#T}](./crm-deal-recurring-add.md)
- [{#T}](./crm-deal-recurring-update.md)
- [{#T}](./crm-deal-recurring-expose.md)