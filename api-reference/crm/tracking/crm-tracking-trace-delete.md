# Delete Sales Intelligence Trace crm.tracking.trace.delete

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user can delete a trace
> - a user with permission to modify the object can delete the trace binding to the object

The method `crm.tracking.trace.delete` removes a Sales Intelligence trace.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Identifier of the Sales Intelligence trace.

To fully delete the trace, permissions to modify all related objects are required.

The `id` can be obtained using the method [crm.tracking.trace.add](./crm-tracking-trace-add.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of deleting a Sales Intelligence trace, where:
- `id` — identifier of the trace

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 125
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/crm.tracking.trace.delete.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 125,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/crm.tracking.trace.delete.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.tracking.trace.delete',
    		{
    			id: 125
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
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.tracking.trace.delete',
                [
                    'id' => 125,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting trace: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.tracking.trace.delete',
        {
            id: 125
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'crm.tracking.trace.delete',
        [
            'id' => 125,
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1775119058,
        "finish": 1775119058.707133,
        "duration": 0.7071330547332764,
        "processing": 0,
        "date_start": "2026-04-02T11:37:38+02:00",
        "date_finish": "2026-04-02T11:37:38+02:00",
        "operating_reset_at": 1775119658,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`null`](../../data-types.md) | The method does not return data in the response.

If no REST error occurred during the call, the `result` field returns `null`.

Upon successful deletion of the binding, the value is cleared in the "Sales Intelligence" field of the entity to which the trace was bound ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Parameter `id` required."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Parameter `id` required. | The `id` parameter is missing or an empty value was provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-tracking-trace-add.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-lead.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-contact.md)