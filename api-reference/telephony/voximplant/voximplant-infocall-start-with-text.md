# Initiate an Auto-Call with the Voice Automation Rule voximplant.infocall.startwithtext

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`call`](../../scopes/permissions.md)
>
> Who can execute the method: a user with Outgoing Call — Execute access permission

The method `voximplant.infocall.startwithtext` initiates an auto-call and plays the specified text to the recipient using speech synthesis.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FROM_LINE***
[`string`](../../data-types.md) | Identifier of the outgoing line from which the call is initiated.

A list of available lines can be obtained using the [voximplant.line.get](./lines/voximplant-line-get.md) method.

Application lines of the form `REST_APP:<APP_ID>` and external application lines are not supported: the method returns the `Could not find config for number` error. The `LINK_BASE_NUMBER` line cannot be used ||
|| **TO_NUMBER***
[`string`](../../data-types.md) | The number to which the call should be made.

For a SIP line, the number is passed unchanged. For other lines, all characters except digits and `+`, `#`, `*`, `,`, `;` are removed from the number. If the resulting number is empty, the method returns the `Phone number is not correct` error ||
|| **TEXT_TO_PRONOUNCE***
[`string`](../../data-types.md) | The text that will be spoken to the recipient. ||
|| **VOICE**
[`string`](../../data-types.md) | Identifier of the voice for speech synthesis.

If not provided, the default voice for the request language is used: `ruinternalfemale` for `ru`, `usenglishmale` for `en`, `eurgermanfemale` for `de`. For languages without a voice of their own, `usenglishmale` is used.

Speech synthesis uses medium rate and volume; they cannot be configured via the REST API

A list of available voices can be obtained using the [voximplant.tts.voices.get](./voximplant-tts-voices-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FROM_LINE":"reg151083","TO_NUMBER":"+19991234567","TEXT_TO_PRONOUNCE":"Good afternoon. This is a reminder for your appointment.","VOICE":"usenglishmale"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.infocall.startwithtext
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FROM_LINE":"reg151083","TO_NUMBER":"+19991234567","TEXT_TO_PRONOUNCE":"Good afternoon. This is a reminder for your appointment.","VOICE":"usenglishmale","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.infocall.startwithtext
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type InfocallStartWithTextResult = {
      RESULT: boolean
      CALL_ID: string
    }

    try {
      const response = await $b24.actions.v2.call.make<InfocallStartWithTextResult>({
        method: 'voximplant.infocall.startwithtext',
        params: {
          FROM_LINE: 'reg151083',
          TO_NUMBER: '79991234567',
          TEXT_TO_PRONOUNCE: 'Good afternoon. This is a reminder for your appointment.',
          VOICE: 'usenglishmale',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Infocall started:', result.RESULT, 'Call ID:', result.CALL_ID)
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
      async function startInfocallWithText() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.infocall.startwithtext',
            params: {
              FROM_LINE: 'reg151083',
              TO_NUMBER: '79991234567',
              TEXT_TO_PRONOUNCE: 'Good afternoon. This is a reminder for your appointment.',
              VOICE: 'usenglishmale',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Infocall started:', result.RESULT, 'Call ID:', result.CALL_ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', startInfocallWithText)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.voximplant.infocall.startwithtext(
            from_line='reg151083',
            to_number='79991234567',
            text_to_pronounce='Good afternoon. This is a reminder about your appointment',
            voice='usenglishmale',
        ).response
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
        $response = $b24Service
            ->core
            ->call(
                'voximplant.infocall.startwithtext',
                [
                    'FROM_LINE' => 'reg151083',
                    'TO_NUMBER' => '+19991234567',
                    'TEXT_TO_PRONOUNCE' => 'Good afternoon. This is a reminder for your appointment.',
                    'VOICE' => 'usenglishmale',
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
        'voximplant.infocall.startwithtext',
        {
            FROM_LINE: 'reg151083',
            TO_NUMBER: '+19991234567',
            TEXT_TO_PRONOUNCE: 'Good afternoon. This is a reminder for your appointment.',
            VOICE: 'usenglishmale'
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
        'voximplant.infocall.startwithtext',
        [
            'FROM_LINE' => 'reg151083',
            'TO_NUMBER' => '+19991234567',
            'TEXT_TO_PRONOUNCE' => 'Good afternoon. This is a reminder for your appointment.',
            'VOICE' => 'usenglishmale',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "voximplant.infocall.startwithtext", b24.Params{
    	"FROM_LINE":         "reg151083",
    	"TO_NUMBER":         "79991234567",
    	"TEXT_TO_PRONOUNCE": "Good afternoon. This is a reminder for your appointment.",
    	"VOICE":             "usenglishmale",
    })
    if err != nil {
    	return fmt.Errorf("voximplant.infocall.startwithtext: %w", err)
    }

    var item struct {
    	Result bool   `json:"RESULT"`
    	CallID string `json:"CALL_ID"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Result, item.CallID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "RESULT": true,
        "CALL_ID": "infocall.4aa8f8e35bb6ece352fec0537b58ac42.1773740701"
    },
    "time": {
        "start": 1773740709,
        "finish": 1773740710.094622,
        "duration": 1.0946218967437744,
        "processing": 1,
        "date_start": "2026-03-17T12:45:09+01:00",
        "date_finish": "2026-03-17T12:45:10+01:00",
        "operating_reset_at": 1773741309,
        "operating": 0.14156007766723633
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing the result of the auto-call initiation [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **RESULT**
[`boolean`](../../data-types.md) | Indicator of successful initiation.

`true` — auto-call successfully initiated. ||
|| **CALL_ID**
[`string`](../../data-types.md) | Identifier of the call ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Infocall limit for this month is exceeded"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Making infocall using LINK_BASE_NUMBER is not allowed` | Auto-call cannot be initiated through a service line ||
|| `400` | Empty value | `Could not find config for number <LINE_ID>` | Configuration for the specified line not found ||
|| `400` | Empty value | `Infocall limit for this month is exceeded` | Monthly limit for auto-calls exceeded. The limit applies on the free telephony plan; SIP lines with an active SIP connector have no limit ||
|| `400` | Empty value | `Phone number is not correct` | Invalid number in `TO_NUMBER` ||
|| `400` | Empty value | `Infocall failure` | Error while initiating the auto-call ||
|| `403` | `ACCESS_DENIED` | `Access denied!` | Insufficient permissions to make outgoing calls ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./lines/voximplant-line-get.md)
- [{#T}](./voximplant-callback-start.md)
- [{#T}](./voximplant-infocall-start-with-sound.md)
- [{#T}](./voximplant-tts-voices-get.md)