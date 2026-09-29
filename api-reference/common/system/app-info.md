# Show information about the app app.info

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `app.info` returns information about the application: its status, version, payment period, and Bitrix24 plan. See also the [response when called via a webhook](#webhook).

## Method Parameters

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AppInfoResult = {
      ID: number
      CODE: string
      VERSION: number
      STATUS: string
      INSTALLED: boolean
      PAYMENT_EXPIRED: string
      DAYS: number | null
      LANGUAGE_ID: string
      LICENSE: string
      LICENSE_PREVIOUS?: string
      LICENSE_TYPE?: string
      LICENSE_FAMILY?: string
    }

    try {
      const response = await $b24.actions.v2.call.make<AppInfoResult>({
        method: 'app.info',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.CODE, result.STATUS, result.LICENSE)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getAppInfo() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'app.info',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.CODE, result.STATUS, result.LICENSE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAppInfo)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.app.info().response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php        
    try {
        $applicationInfoResult = $serviceBuilder->getMainScope()->main()->getApplicationInfo();
        $itemResult = $applicationInfoResult->applicationInfo();
        print("ID: " . $itemResult->ID . PHP_EOL);
        print("Code: " . $itemResult->CODE . PHP_EOL);
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

- BX24.js

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

- PHP CRest

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "app.info", nil)
    if err != nil {
    	return fmt.Errorf("app.info: %w", err)
    }

    var item struct {
    	ID             b24.ID `json:"ID"`
    	Code           string `json:"CODE"`
    	Version        int    `json:"VERSION"`
    	Status         string `json:"STATUS"`
    	Installed      bool   `json:"INSTALLED"`
    	PaymentExpired string `json:"PAYMENT_EXPIRED"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Code)
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
[`object`](../../data-types.md) | Information about the application [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Local identifier of the application in Bitrix24 ||
|| **CODE**
[`string`](../../data-types.md) | Application code ||
|| **VERSION**
[`integer`](../../data-types.md) | Installed version of the application ||
|| **STATUS**
[`string`](../../data-types.md) | Status of the application:

- `L` — local application
- `F` — free mass-market application
- `D` — demo version of a mass-market application
- `T` — trial version of a mass-market application, time-limited
- `P` — paid mass-market application ||
|| **INSTALLED**
[`boolean`](../../data-types.md) | Whether the application installation is complete. If `false`, the application is only available to Bitrix24 administrators and must report the end of installation by calling [BX24.installFinish](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md). While the value is `false`, Bitrix24 does not deliver events to the application and does not show its placements, even if [event.bind](../../events/event-bind.md) and [placement.bind](../../widgets/placement-bind.md) succeeded ||
|| **PAYMENT_EXPIRED**
[`string`](../../data-types.md) | Whether the paid period has expired: `Y` or `N` ||
|| **DAYS**
[`integer`](../../data-types.md) | Number of days until the end of the paid or trial period. After the period ends, a negative number.

`null` if the period is not limited, for example, for a free or local application ||
|| **LANGUAGE_ID**
[`string`](../../data-types.md) | Default language of the Bitrix24 site, for example `de` ||
|| **LICENSE**
[`string`](../../data-types.md) | Bitrix24 plan with a region prefix, for example `de_std`. For plans whose composition changed while the name stayed the same, such as CRM+, Team, and Company, this field does not show which plan is active. Examples of values:

- `de_project` — Project plan
- `de_basic` — Basic plan
- `de_std` — Standard plan
- `de_pro100` — Professional plan
- `de_ent250` — Enterprise 250
- `de_ent500` — Enterprise 500
- `de_ent1000` — Enterprise 1000
- `de_ent2000` — Enterprise 2000
- `de_ent10000` — Enterprise 10000

In on-premise Bitrix24, it is the string `<language>_selfhosted`, for example, `en_selfhosted` ||
|| **LICENSE_PREVIOUS**
[`string`](../../data-types.md) | The plan that was active before the demo mode, in the `LICENSE` format. Returned only if Bitrix24 is currently in the plan's demo mode ||
|| **LICENSE_TYPE**
[`string`](../../data-types.md) | Plan identifier without the region prefix, for example `ent10000`. Cloud Bitrix24 only ||
|| **LICENSE_FAMILY**
[`string`](../../data-types.md) | Plan family, for example `ent`. Cloud Bitrix24 only ||
|#

{% note info "" %}

When the paid period has expired, the `PAYMENT_EXPIRED` field equals `Y`. If the end date of the period is known, the `DAYS` field contains a negative number — the number of days since the period ended.

{% endnote %}

### Response When Called via a Webhook {#webhook}

When called via a webhook, the method does not return application data; instead, it returns the webhook's scopes and the Bitrix24 plan:

```json
{
    "result": {
        "SCOPE": [
            "crm",
            "user"
        ],
        "LICENSE": "de_std"
    },
    "time": {
        "start": 1790665588,
        "finish": 1790665588.741944,
        "duration": 0.7419440746307373,
        "processing": 0,
        "date_start": "2026-09-29T09:06:28+02:00",
        "date_finish": "2026-09-29T09:06:28+02:00",
        "operating_reset_at": 1790666188,
        "operating": 0.11118412017822266
    }
}
```

#|
|| **Name**
`type` | **Description** ||
|| **SCOPE**
[`string[]`](../../data-types.md) | Webhook scope codes, as in the [scope](./scope.md) method ||
|| **LICENSE**
[`string`](../../data-types.md) | Bitrix24 plan, in the same format as the `LICENSE` field above ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied! Application context required"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! Application context required | The request is authorized by a user session rather than by an application or a webhook ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./access-name.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)