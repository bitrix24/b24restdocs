# Determine the Current CRM Operating Mode crm.settings.mode.get

> Method name: **crm.settings.mode.get**
>
> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method returns the current settings for the CRM operating mode: **classic CRM mode** (with leads) or **simple CRM mode** (without leads).

This mode affects a number of CRM operation scenarios, and for better understanding, we recommend reading the [relevant article](https://helpdesk.bitrix24.com/open/24207198/) in the user documentation.

## Method Parameters

The method is called without parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.settings.mode.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.settings.mode.get
    ```

- JS

    ```js
    BX24.callMethod("crm.settings.mode.get", {}, result => {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    });
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.settings.mode.get',
        []
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
    "result": 1,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Returns the value defined in [crm.enum.settings.mode](./auxiliary/enum/crm-enum-settings-mode.md) ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

{% include [system errors](../../_includes/system-errors.md) %}