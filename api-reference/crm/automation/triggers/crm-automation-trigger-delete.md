# Delete Trigger crm.automation.trigger.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator with access to CRM in the application context

This method deletes a trigger.

The method can only be executed in the application context.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Internal unique (within the application) identifier of the trigger. Must match the pattern `[a-z0-9\.\-_]` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"c5u4m"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automation.trigger.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"c5u4m","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.automation.trigger.delete',
        {
            "CODE": 'c5u4m'
        },
        function(result) 
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.automation.trigger.delete',
        [
            'CODE' => 'c5u4m'
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
    "result":true,
    "time":{
        "start":1718891973.429101,
        "finish":1718891986.721889,
        "duration":13.292788028717041,
        "processing":13.012810945510864,
        "date_start":"2024-06-20T13:59:33+00:00",
        "date_finish":"2024-06-20T13:59:46+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if deleted successfully ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"",
    "error_description":"Trigger not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied. | User did not pass the preliminary access rights check for CRM ||
|| ACCESS_DENIED | Access denied! Admin permissions required | Administrator rights check failed ||
|| ACCESS_DENIED | Access denied! Application context required | Method called outside the application context ||
|| Empty string | Empty trigger code! | Empty `CODE` parameter ||
|| Empty string | Wrong trigger code! | `CODE` parameter does not match the pattern `[a-z0-9\.\-_]` ||
|| Empty string | Trigger not found | Trigger not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-automation-trigger-add.md)
- [{#T}](./crm-automation-trigger-execute.md)
- [{#T}](./crm-automation-trigger-list.md)