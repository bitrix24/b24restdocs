# Get Call Follow-up call.followup.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`call`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the call

{% note info "" %}

The method belongs to REST 3.0. Calling features and the response format of the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

The `call.followup.get` method returns the follow-up of a single call by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **callId***
[`integer`](../../data-types.md) | Call identifier.

The identifier can be obtained using the [call.followup.list](./call-followup-list.md) method ||
|| **select**
[`array`](../../data-types.md) | A list of fields and nested paths to be returned in the response.

If the parameter is not provided, the method returns all fields from the [Root object](./fields.md#root-object) section. Missing data is set to `null`.

If an empty array is provided, the method returns only basic metadata: `callId`, `callType`, `initiatorId`, `startDate`, `endDate`, `durationSeconds`.

If a list of fields is provided, the method returns only the listed fields and always adds `callId`. See the full list of fields in the [Follow-up call fields](./fields.md#select-paths) article ||
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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/call.followup.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"callId":12345,"mentionFormat":"html"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/call.followup.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"callId":12345,"mentionFormat":"html","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/call.followup.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FollowUpGetResult = {
      item: {
        callId: number
        callType?: number
        initiatorId?: number
        startDate?: string
        endDate?: string
        durationSeconds?: number
        uuid?: string
        language?: string
        version?: number
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
      const response = await $b24.actions.v3.call.make<FollowUpGetResult>({
        method: 'call.followup.get',
        params: {
          callId: 12345,
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
      async function getFollowUp() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'call.followup.get',
            params: {
              callId: 12345,
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

      document.addEventListener('DOMContentLoaded', getFollowUp)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'call.followup.get',
                [
                    'callId' => 12345,
                    'mentionFormat' => 'html',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'call.followup.get',
        {
            callId: 12345,
            mentionFormat: 'html'
        },
        function(result) {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'call.followup.get',
        [
            'callId' => 12345,
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
    res, err := client.Core().Call(ctx, "call.followup.get", b24.Params{
    	"callId":        12345,
    	"mentionFormat": "html",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("call.followup.get: %w", err)
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
            "callId": 12345,
            "callType": 1,
            "initiatorId": 7,
            "startDate": "2026-01-15T10:00:00+00:00",
            "endDate": "2026-01-15T10:42:00+00:00",
            "durationSeconds": 2520,
            "uuid": "bb085e5d-5160-4a63-9ac4-152248046c39",
            "language": "de",
            "version": 3,
            "participants": [ /* ParticipantDto */ ],
            "outcomes": ["transcription", "overview", "summary", "insights", "evaluation"],
            "createdAt": "2026-01-15T11:05:00+00:00",
            "tracks": [ /* TrackDto */ ],
            "transcription": { "language": "de", "segments": [ /* TranscriptionSegmentDto */ ] },
            "overview": { "topic": "Sprint Planning", "actionItems": [ /* ... */ ] },
            "summary": { "segments": [ /* SummarySegmentDto */ ] },
            "insights": { "speakerEvaluationAvailable": true, "speakerAnalysis": [ /* ... */ ] },
            "evaluation": { "efficiencyValue": 75, "criteria": { /* ... */ } }
        }
    },
    "time": {
        "start": 1784017027,
        "finish": 1784017027.356922,
        "duration": 0.356921911239624,
        "processing": 0,
        "date_start": "2026-07-14T11:17:07+03:00",
        "date_finish": "2026-07-14T11:17:07+03:00",
        "operating_reset_at": 1784017627,
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
[`object`](../../data-types.md) | Follow-up object. The field composition depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "access_denied",
        "message": "No access to Follow-up data"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check that the application or webhook has the `call` scope ||
|#

Error Code: `access_denied`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `callId` | No access to Follow-up data | Check that the user has access to the call or is part of the associated chat ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `callId` | Call not found | Provide an existing call identifier. For a user without rights to the call, the method will return an access error ||
|#

#### Errors in the `select` Parameter

Error Code: `invalid_select_field`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select` | Provide a field from the list of available values `select` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `callId` | A required parameter was not provided or an incorrect type was provided | Provide `callId` as an integer ||
|| `mentionFormat` | The provided value is not from the list of allowed formats | Provide `bb`, `html`, or `none` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./call-followup-list.md)
- [{#T}](./fields.md)
- [{#T}](./call-followup-field-list.md)
- [{#T}](./call-followup-field-get.md)
- [{#T}](./index.md)

<!-- Generated by skill-1-gen-docs from source: api-reference/telephony/doc.md, C:\Users\g.m.tagirova\original-bitrix\modules\call\lib\Controller\FollowUp.php -->
<!-- Generated: 2026-07-14 -->
<!-- Requires verification: no -->
