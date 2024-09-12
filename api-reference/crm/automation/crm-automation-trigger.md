# Activate the trigger crm.automation.trigger

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to modify the target object `target` 

Activates the Webhook trigger configured in the CRM automation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **target***
[`string`](../../data-types.md) | The target object for automation, specified in the format [`TYPENAME_ID`](../../data-types.md#object_type) (for example, `LEAD_25`)
||
|| **code**
[`string`](../../data-types.md) | A unique symbolic code for the trigger configured in Automation for a specific status/stage of the document. The `code` parameter can be obtained from the trigger settings ||
|#

{% note info %}

In rare cases, multiple triggers may be found for the specified `target` object. This occurs if:
- the `code` is not provided in the request and there are old triggers on the account that do not have a `code`
- a `code` is provided that is the same for multiple triggers
In such cases, the first trigger that sets an earlier status for the CRM object will be executed.

It is also important to consider that triggers have "Conditions" and the option "Allow reverting to the previous status," which affect whether the trigger will execute or not.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"target":"DEAL_57","code":"c5u4m"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automation.trigger
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"target":"DEAL_57","code":"c5u4m","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.automation.trigger",
        {
            target: 'DEAL_57',
            code: 'c5u4m',
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
        'crm.automation.trigger',
        [
            'target' => 'DEAL_57',
            'code' => 'c5u4m'
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
        "start":1718809827.810153,
        "finish":1718809828.541046,
        "duration":0.7308928966522217,
        "processing":0.09834408760070801,
        "date_start":"2024-06-19T15:10:27+00:00",
        "date_finish":"2024-06-19T15:10:28+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The result of activating the trigger ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"",
    "error_description":"Target is not set."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Target is not set. | The required parameter `target` was not provided ||
|| Empty string | Incorrect target format. | The `target` parameter was not provided in the required format (Required format `TYPENAME_ID`) ||
|| Empty string | Target is not found. | An incorrect `TYPENAME` was provided in the `target` parameter ||
|| `ACCESS_DENIED` | Access denied! There are no permissions to update the entity. | The user did not pass the permission check to trigger the automation.  ||
|| Empty string | Access denied. | The user did not pass the preliminary permission check for CRM access ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./triggers/index.md)