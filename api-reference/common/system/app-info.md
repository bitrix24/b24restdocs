# Show Application Information app.info

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `app.info` returns information about the application.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/app.info
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/app.info
    ```

- JS

    ```js
    BX24.callMethod(
        "app.info",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'app.info',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- B24-PHP-SDK

    ```php        
    try {
        $applicationInfoResult = $serviceBuilder->getMainScope()->main()->getApplicationInfo();
        $itemResult = $applicationInfoResult->applicationInfo();
        print("ID: " . $itemResult->ID . PHP_EOL);
        print("Code: " . $itemResult->CODE . PHP_EOL);
        print("Scope: " . json_encode($itemResult->SCOPE, JSON_THROW_ON_ERROR) . PHP_EOL);
        print("Version: " . $itemResult->VERSION . PHP_EOL);
        print("Status: " . $itemResult->getStatus()->getStatusCode() . PHP_EOL);
        print("Installed: " . ($itemResult->INSTALLED ? 'true' : 'false') . PHP_EOL);
        print("Payment Expired: " . $itemResult->PAYMENT_EXPIRED . PHP_EOL);
        print("Days: " . $itemResult->DAYS . PHP_EOL);
        print("License: " . $itemResult->LICENSE . PHP_EOL);
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . PHP_EOL);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": 5,
        "CODE": "telefum24.kp10",
        "VERSION": 4,
        "STATUS": "F",
        "INSTALLED": true,
        "PAYMENT_EXPIRED": "N",
        "DAYS": null,
        "LANGUAGE_ID": "de",
        "LICENSE": "de_ent10000",
        "LICENSE_TYPE": "ent10000",
        "LICENSE_FAMILY": "ent"
    },
    "time": {
        "start": 1722841503.0585,
        "finish": 1722841503.09885,
        "duration": 0.0403509140014648,
        "processing": 0.00533103942871094,
        "date_start": "2024-08-05T07:05:03+00:00",
        "date_finish": "2024-08-05T07:05:03+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The object contains information about the application:

- `ID` — local identifier of the application on the account 
- `CODE` — application code 
- `VERSION` — installed version of the application 
- `STATUS` — status of the application. Possible values:
    - `F` (Free) — free
    - `D` (Demo) — demo version
    - `T` (Trial) — trial version (time-limited)
    - `P` (Paid) — paid application
    - `L` (Local) — on-premise application
    - `S` (Subscription) — subscription application 
- `INSTALLED` — [true\|false] status of the application's installation. If the application is not installed, it is only available to account administrators and should signal the completion of installation by calling [BX24.installFinish](../../bx24-js-sdk/system-functions/bx24-install-finish.md)
- `PAYMENT_EXPIRED` — [Y\|N] flag indicating whether the paid period or trial period has expired
- `DAYS` — number of days remaining until the end of the paid period or trial period
- `LICENSE` — designation of the plan with the region indicated as a prefix. Consists of the base language of the account and the identifier of the plan. In cases where the plans have changed while retaining the public name (like CRM+, Team, and Company), it is not possible to determine which plan is currently active based on this field. Examples of possible values:
    - `de_project` — Project plan
    - `de_basic` — Basic plan
    - `de_std` — Standard plan
    - `de_pro100` — Professional plan
    - `de_ent250` — Enterprise 250
    - `de_ent500` — Enterprise 500
    - `de_ent1000` — Enterprise 1000
    - `de_ent2000` — Enterprise 2000
    - `de_ent10000` — Enterprise 10000 ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

{% note info "" %}

After the paid period expires, the application will continue to operate during the grace period, after which it will automatically switch to demo mode or be blocked. At this point, the value of the `PAYMENT_EXPIRED` flag will be `Y`, and the `DAYS` field will contain a negative number.

{% endnote %}

## Error Handling

HTTP status: **400**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Access denied! Application context required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Access denied! Application context required | Method called outside the application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)