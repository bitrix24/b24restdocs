# Get Bitrix24 Sync Call Follow-up syncconnector.followup.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`syncconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Follow-up

{% note info "" %}

The method belongs to REST 3.0. Calling features and the response format of the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

The `syncconnector.followup.get` method returns a Bitrix24 Sync call Follow-up by the identifier of its detailed analysis page.

A regular user receives the Follow-up if they spoke in the call part the page belongs to or if they are a member of the call chat. In an open chat, the Follow-up is visible only to chat members, not to all employees. An administrator receives the Follow-up of any detailed analysis page.

The parameters and response fields are the same as for [call.followup.get](./call-followup-get.md). For value differences in Sync calls, see the section [Field Differences in Bitrix24 Sync](./fields.md#sync). [call.followup.get](./call-followup-get.md) also returns the same Follow-up if you pass `10000000000` plus the page identifier in `callId`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **callId***
[`integer`](../../data-types.md) | Detailed analysis page identifier. This is not a call identifier: a Sync call can have several pages, one for each call part.

You can retrieve the identifier with the [syncconnector.followup.list](./syncconnector-followup-list.md) method. The "Detailed analysis" link in the chat also contains it ||
|| **select**
[`array`](../../data-types.md) | A list of fields and nested paths to be returned in the response.

What the method returns:

- not passed — all fields from the [Root Object](./fields.md#root-object) section except `source`, which must be passed in `select` explicitly. Fields without data are returned as `null`
- empty array — only the fields `callId`, `callType`, `initiatorId`, `startDate`, `endDate`, `durationSeconds`
- list of fields — only the listed fields, and always `callId`

For a full list of fields, see the article [Call Follow-up Fields](./fields.md#select-paths) ||
|| **mentionFormat**
[`string`](../../data-types.md) | Format for user mentions in AI text fields.

Possible values:

- `bb` — BBCode format
- `html` — HTML format
- `none` — plain text without mention markup

Default: `bb` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

Calling the new API differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/syncconnector.followup.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"callId":128,"mentionFormat":"html"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/syncconnector.followup.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"callId":128,"mentionFormat":"html","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/syncconnector.followup.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SyncFollowUpGetResult = {
      item: {
        callId: number
        callType?: number
        chatId?: number
        initiatorId?: null
        startDate?: string
        endDate?: string | null
        durationSeconds?: number | null
        uuid?: string
        language?: string | null
        version?: number | null
        participants?: unknown[]
        outcomes?: string[]
        createdAt?: string
        tracks?: unknown[]
        transcription?: unknown
        overview?: unknown
        summary?: unknown
        insights?: unknown
        evaluation?: unknown
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<SyncFollowUpGetResult>({
        method: 'syncconnector.followup.get',
        params: {
          callId: 128,
          mentionFormat: 'html',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item)
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
      async function getSyncFollowUp() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'syncconnector.followup.get',
            params: {
              callId: 128,
              mentionFormat: 'html',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSyncFollowUp)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made methods for [syncconnector.followup.*](./index.md#sync-methods), so call the method via `call_method` and add it to `api_v3_methods`, as shown in the example.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "syncconnector.followup.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "callId": 128,
                "mentionFormat": "html",
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'syncconnector.followup.get',
                [
                    'callId' => 128,
                    'mentionFormat' => 'html',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'syncconnector.followup.get',
        {
            callId: 128,
            mentionFormat: 'html'
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'syncconnector.followup.get',
        [
            'callId' => 128,
            'mentionFormat' => 'html',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "syncconnector.followup.get", b24.Params{
    	"callId":        128,
    	"mentionFormat": "html",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("syncconnector.followup.get: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```jsonc
{
    "result": {
        "item": {
            "callId": 128,
            "callType": 1,
            "chatId": 2048,
            "initiatorId": null,
            "startDate": "2026-11-12T09:00:00+00:00",
            "endDate": "2026-11-12T09:42:00+00:00",
            "durationSeconds": 2520,
            "uuid": "sync-call-key",
            "language": "de",
            "version": 3,
            "participants": [ /* ParticipantDto */ ],
            "outcomes": ["transcription", "overview", "summary", "insights", "evaluation"],
            "createdAt": "2026-11-12T09:50:00+00:00",
            "tracks": [ /* TrackDto */ ],
            "transcription": { "language": "de", "segments": [ /* TranscriptionSegmentDto */ ] },
            "overview": { "topic": "Sprint planning", "actionItems": [ /* ... */ ] },
            "summary": { "segments": [ /* SummarySegmentDto */ ] },
            "insights": { "speakerEvaluationAvailable": true, "speakerAnalysis": [ /* ... */ ] },
            "evaluation": { "efficiencyValue": 75, "criteria": { /* ... */ } }
        }
    },
    "time": {
        "start": 1794476996,
        "finish": 1794476996.873855,
        "duration": 0.8738551139831543,
        "processing": 0,
        "date_start": "2026-11-12T12:49:56+03:00",
        "date_finish": "2026-11-12T12:49:56+03:00",
        "operating_reset_at": 1794477540,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../data-types.md) | Follow-up object. The field composition depends on `select`. For field descriptions, see the article [Call Follow-up Fields](./fields.md#root-object); for differences, see the section [Field Differences in Bitrix24 Sync](./fields.md#sync) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "access_denied",
        "message": "No access to Follow-Up data"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient access permissions: required scope is missing | Check that the application or webhook has the `syncconnector` scope ||
|#

Error Code: `access_denied`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `callId` | No access to Follow-Up data | The method returns this error both for a nonexistent `callId` and for another user's Follow-up. An administrator also receives it if there is no page with this `callId`. Take `callId` from the [syncconnector.followup.list](./syncconnector-followup-list.md) response: it contains only the Follow-ups available to the user ||
|#

#### Errors in the `select` Parameter

Error Code: `invalid_select_field`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Field not allowed in select: `#FIELD#` | Pass a field from the [List of Available Values](./fields.md#select-paths) ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `callId` | Required field `callId` is missing | Pass `callId` ||
|| `callId` | This request requires data type `int` in field `callId` | Pass `callId` as an integer ||
|| `mentionFormat` | Field value is not one of the possible values | Pass `bb`, `html`, or `none` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./syncconnector-followup-list.md)
- [{#T}](./syncconnector-followup-field-list.md)
- [{#T}](./syncconnector-followup-field-get.md)
- [{#T}](./fields.md)
- [{#T}](./index.md)
