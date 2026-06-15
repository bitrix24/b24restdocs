# Create SIP Line voximplant.sip.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with the Management of numbers — modification access permission

The method `voximplant.sip.add` creates a new SIP line associated with an application.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SERVER***
[`string`](../../../data-types.md) | Address of the SIP registration server ||
|| **LOGIN***
[`string`](../../../data-types.md) | Login for connecting to the server ||
|| **PASSWORD**
[`string`](../../../data-types.md) | Password for connecting to the server. Maximum length — 100 characters.

Not required for calling the method, but necessary for working registration with the operator ||
|| **TYPE**
[`string`](../../../data-types.md) | Type of PBX.

Possible values:

- `cloud` - cloud PBX
- `office` - office PBX

Default: `cloud` ||
|| **TITLE**
[`string`](../../../data-types.md) | Name of the connection.

If the parameter is not provided, the interface will display a system name based on the connection type:

- for `cloud` — Cloud PBX (ID)
- for `office` — Office PBX (ID)

where `ID` is the internal identifier of the SIP line record.

The `TITLE` field in the method response will be empty in this case ||
|#

{% note info "" %}

You can connect no more than 10 cloud SIP lines. Exceeding this limit will return the error `MAX_CLOUD_PBX`

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TYPE":"cloud","TITLE":"SIP line 1","SERVER":"sip.provider.com","LOGIN":"sip_user","PASSWORD":"secret"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.sip.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TYPE":"cloud","TITLE":"SIP line 1","SERVER":"sip.provider.com","LOGIN":"sip_user","PASSWORD":"secret","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.sip.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SipAddResult = {
      ID: string
      TYPE: string
      CONFIG_ID: string
      REG_ID?: number
      TITLE: string
      SERVER: string
      LOGIN: string
      PASSWORD: string
      AUTH_USER: string | null
      OUTBOUND_PROXY: string | null
      DETECT_LINE_NUMBER: string
      LINE_DETECT_HEADER_ORDER: string
      REGISTRATION_STATUS_CODE: number | null
      REGISTRATION_ERROR_MESSAGE: string | null
      INCOMING_SERVER?: string
      INCOMING_LOGIN?: string
      INCOMING_PASSWORD?: string
    }

    try {
      const response = await $b24.actions.v2.call.make<SipAddResult>({
        method: 'voximplant.sip.add',
        params: {
          TYPE: 'cloud',
          TITLE: 'SIP line 1',
          SERVER: 'sip.provider.com',
          LOGIN: 'sip_user',
          PASSWORD: 'secret',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created SIP line:', result.ID, result.TYPE, result.TITLE)
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
      async function addSipLine() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.sip.add',
            params: {
              TYPE: 'cloud',
              TITLE: 'SIP line 1',
              SERVER: 'sip.provider.com',
              LOGIN: 'sip_user',
              PASSWORD: 'secret',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created SIP line:', result.ID, result.TYPE, result.TITLE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addSipLine)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'voximplant.sip.add',
                [
                    'TYPE' => 'cloud',
                    'TITLE' => 'SIP line 1',
                    'SERVER' => 'sip.provider.com',
                    'LOGIN' => 'sip_user',
                    'PASSWORD' => 'secret',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.sip.add',
        {
            TYPE: 'cloud',
            TITLE: 'SIP line 1',
            SERVER: 'sip.provider.com',
            LOGIN: 'sip_user',
            PASSWORD: 'secret'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'voximplant.sip.add',
        [
            'TYPE' => 'cloud',
            'TITLE' => 'SIP line 1',
            'SERVER' => 'sip.provider.com',
            'LOGIN' => 'sip_user',
            'PASSWORD' => 'secret',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


## Response Handling

HTTP Status: **200**

### Example Response for Creating a Cloud PBX

```json
{
    "result": {
        "ID": "5",
        "TYPE": "cloud",
        "CONFIG_ID": "5",
        "REG_ID": 151082,
        "TITLE": "SIP line 1",
        "SERVER": "sip.provider.com",
        "LOGIN": "sip_user",
        "PASSWORD": "secret",
        "AUTH_USER": null,
        "OUTBOUND_PROXY": null,
        "DETECT_LINE_NUMBER": "N",
        "LINE_DETECT_HEADER_ORDER": "diversion;to",
        "REGISTRATION_STATUS_CODE": null,
        "REGISTRATION_ERROR_MESSAGE": null
    },
    "time": {
        "start": 1773654128,
        "finish": 1773654129.048472,
        "duration": 1.0484719276428223,
        "processing": 1,
        "date_start": "2026-03-16T12:42:08+02:00",
        "date_finish": "2026-03-16T12:42:09+02:00",
        "operating_reset_at": 1773654728,
        "operating": 0.2883341312408447
    }
}
```

### Example Response for Creating an Office PBX

```json
{
    "result": {
        "ID": "7",
        "TYPE": "office",
        "CONFIG_ID": "7",
        "SERVER": "office.provider.local",
        "LOGIN": "office_user",
        "PASSWORD": "secret",
        "INCOMING_SERVER": "ip.b24-6058-1587535982.bitrixphone.com",
        "INCOMING_LOGIN": "sip7",
        "INCOMING_PASSWORD": "71747503265fb091223eb31776a4a225",
        "AUTH_USER": null,
        "OUTBOUND_PROXY": null,
        "DETECT_LINE_NUMBER": "N",
        "LINE_DETECT_HEADER_ORDER": "diversion;to",
        "REGISTRATION_STATUS_CODE": null,
        "REGISTRATION_ERROR_MESSAGE": null,
        "TITLE": "Office PBX 1"
    },
    "time": {
        "start": 1773654928,
        "finish": 1773654928.708338,
        "duration": 0.7083380222320557,
        "processing": 0,
        "date_start": "2026-03-16T12:55:28+02:00",
        "date_finish": "2026-03-16T12:55:28+02:00",
        "operating_reset_at": 1773655528,
        "operating": 0.24362492561340332
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the created SIP line ||
|| **ID**
[`string`](../../../data-types.md) | Internal identifier of the SIP line record ||
|| **TYPE**
[`string`](../../../data-types.md) | Type of PBX ||
|| **CONFIG_ID**
[`string`](../../../data-types.md) | Identifier of the SIP line configuration ||
|| **REG_ID**
[`integer`](../../../data-types.md) | Identifier of the SIP registration.

Returned when creating a cloud PBX ||
|| **TITLE**
[`string`](../../../data-types.md) | Name of the connection ||
|| **SERVER**
[`string`](../../../data-types.md) | Address of the SIP registration server ||
|| **LOGIN**
[`string`](../../../data-types.md) | Login for connecting to the server ||
|| **PASSWORD**
[`string`](../../../data-types.md) | Password for connecting to the server ||
|| **AUTH_USER**
[`string`](../../../data-types.md) | User for SIP authorization ||
|| **OUTBOUND_PROXY**
[`string`](../../../data-types.md) | Address of the SIP proxy for outgoing connection to the operator or PBX ||
|| **DETECT_LINE_NUMBER**
[`string`](../../../data-types.md) | Indicator for line number detection.

Possible values:

- `Y` — line number detection enabled
- `N` — line number detection disabled ||
|| **LINE_DETECT_HEADER_ORDER**
[`string`](../../../data-types.md) | Order of headers for line number detection ||
|| **REGISTRATION_STATUS_CODE**
[`integer`](../../../data-types.md) | Status code of the SIP line registration ||
|| **REGISTRATION_ERROR_MESSAGE**
[`string`](../../../data-types.md) | Text of the SIP registration error ||
|| **INCOMING_SERVER**
[`string`](../../../data-types.md) | Address of the server for incoming calls.

Returned when creating an office PBX ||
|| **INCOMING_LOGIN**
[`string`](../../../data-types.md) | Login for incoming calls.

Returned when creating an office PBX ||
|| **INCOMING_PASSWORD**
[`string`](../../../data-types.md) | Password for incoming calls.

Returned when creating an office PBX ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CHECK_FIELDS_ERROR",
    "error_description": "Server address not specified"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHECK_FIELDS_ERROR` | `Server address not specified` | Required parameter `SERVER` is missing ||
|| `CHECK_FIELDS_ERROR` | `Login for connecting to the server not specified` | Required parameter `LOGIN` is missing ||
|| `CHECK_FIELDS_ERROR` | `Password for connecting to the server cannot exceed 100 characters` | Exceeded limit for parameter `PASSWORD` ||
|| `TITLE_EXISTS` | `The specified connection name is already registered in the system` | A line with this name already exists ||
|| `MAX_CLOUD_PBX` | `You cannot connect more than 10 virtual PBXs.` | Exceeded limit for cloud SIP lines ||
|| `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to create a SIP line ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-sip-add.md)
- [{#T}](./voximplant-sip-update.md)
- [{#T}](./voximplant-sip-get.md)
- [{#T}](./voximplant-sip-delete.md)
- [{#T}](./voximplant-sip-status.md)
- [{#T}](./voximplant-sip-connector-status.md)