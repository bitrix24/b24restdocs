# Get a List of Bitrix24 Sync Call Follow-up Fields syncconnector.followup.field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`syncconnector`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. Details regarding the invocation features and response format of the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

The `syncconnector.followup.field.list` method returns a list of available Bitrix24 Sync call Follow-up fields. For each field, the method returns a description, such as its name and type. You can pass field names to `select` in the [syncconnector.followup.list](./syncconnector-followup-list.md) and [syncconnector.followup.get](./syncconnector-followup-get.md) methods.

Field descriptions in the response are returned in English.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of description fields to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — filter availability indicator
- `sortable` — sort availability indicator
- `editable` — editability indicator
- `editableGroups` — editable groups
- `multiple` — multiple value indicator
- `elementType` — element type for composite fields

If the parameter is not passed, the method returns all description fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/syncconnector.followup.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","description"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/syncconnector.followup.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","description"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/syncconnector.followup.field.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SyncFollowUpFieldItem = {
      name: string
      type: string
      description: string | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SyncFollowUpFieldListResult = {
      items: SyncFollowUpFieldItem[]
    }

    try {
      const response = await $b24.actions.v3.call.make<SyncFollowUpFieldListResult>({
        method: 'syncconnector.followup.field.list',
        params: {
          select: ['name', 'type', 'description'],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fields count:', result.items.length, 'Fields:', result.items)
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
      async function getSyncFollowUpFieldList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'syncconnector.followup.field.list',
            params: {
              select: ['name', 'type', 'description'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Fields count:', result.items.length, 'Fields:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSyncFollowUpFieldList)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made methods for [syncconnector.followup.*](./index.md#sync-methods), so call the method via `call_method` and add it to `api_v3_methods`, as shown in the example.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "syncconnector.followup.field.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["name", "type", "description"],
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
                'syncconnector.followup.field.list',
                [
                    'select' => ['name', 'type', 'description'],
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
        'syncconnector.followup.field.list',
        {
            select: ['name', 'type', 'description']
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
        'syncconnector.followup.field.list',
        [
            'select' => ['name', 'type', 'description'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "syncconnector.followup.field.list", b24.Params{
    	"select": []string{"name", "type", "description"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("syncconnector.followup.field.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		Name        string `json:"name"`
    		Type        string `json:"type"`
    		Description string `json:"description"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range result.Items {
    	fmt.Println(it.Name, it.Type)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "name": "callId",
                "type": "int",
                "description": "Follow-up page identifier (b_syncconnector_followup_page.ID): the id get accepts and the \"Detailed analysis\" link carries. Always present."
            },
            {
                "name": "source",
                "type": "string",
                "description": "Origin of the follow-up: `call` — a Bitrix24 call, `sync` — a Sync meeting. Opt-in."
            },
            {
                "name": "callType",
                "type": "int",
                "description": "Call type: always 1 (instant). A conventional value: a Sync call has no call type of the portal."
            },
            {
                "name": "chatId",
                "type": "int",
                "description": "Id of the chat the follow-up page belongs to. Opt-in: list this in `select` to receive it."
            },
            {
                "name": "initiatorId",
                "type": "int",
                "description": "Always null: Sync does not tell the portal who initiated the call."
            },
            {
                "name": "startDate",
                "type": "string",
                "description": "ISO 8601 UTC timestamp when the page starts: the start of the window of the call part, without a window the time the page was shown."
            },
            {
                "name": "endDate",
                "type": "string",
                "description": "ISO 8601 UTC timestamp when the window of the call part ended. Null when the page has no window."
            },
            {
                "name": "durationSeconds",
                "type": "int",
                "description": "Duration of the window of the call part in seconds (endDate − startDate), never negative. Null when the page has no window."
            },
            {
                "name": "uuid",
                "type": "string",
                "description": "Key of the Sync call, shared by all its parts; not a UUID. Opt-in: list this in `select` to receive it."
            },
            {
                "name": "language",
                "type": "string",
                "description": "Detected transcription language (BCP-47 / ISO 639-1, e.g. \"ru\", \"en\"). Null when there is no transcription."
            },
            {
                "name": "version",
                "type": "int",
                "description": "Maximum schema version across all stored outcome blocks (transcription, overview, summary, insights, evaluation)."
            },
            {
                "name": "participants",
                "type": "array",
                "description": "Portal users among the speakers of the page, with display data (name, avatar, work position); guests are not listed. Opt-in via `select`."
            },
            {
                "name": "outcomes",
                "type": "array",
                "description": "Names of outcome blocks present for this page: any subset of [transcription, overview, summary, insights, evaluation]."
            },
            {
                "name": "createdAt",
                "type": "string",
                "description": "ISO 8601 UTC timestamp when the follow-up page was shown."
            },
            {
                "name": "tracks",
                "type": "array",
                "description": "Audio of the follow-up stored on Disk, with download URLs; empty when there is none or the viewer cannot read it. Opt-in via `select`."
            },
            {
                "name": "transcription",
                "type": "object",
                "description": "Time-ordered transcription of the call with per-segment speaker attribution."
            },
            {
                "name": "overview",
                "type": "object",
                "description": "AI overview of the meeting: topic, agenda, agreements, action items, takeaways."
            },
            {
                "name": "summary",
                "type": "object",
                "description": "Segmented summary of the call by topic chunks."
            },
            {
                "name": "insights",
                "type": "object",
                "description": "AI insights: per-speaker analysis with region-gated evaluation, meeting strengths/weaknesses, recommendations."
            },
            {
                "name": "evaluation",
                "type": "object",
                "description": "Meeting efficiency evaluation: overall score and individual evaluation criteria."
            }
        ]
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
|| **items**
[`array`](../../data-types.md) | Array of objects with field descriptions. The set of keys in each object depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

For field descriptions, see the article [Call Follow-up Fields](./fields.md); for differences in Sync values, see the section [Field Differences in Bitrix24 Sync](./fields.md#sync).

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"bad\" found in entity \"DtoFieldDto\"."
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

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only the description fields listed for the `select` parameter ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in "select": `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./syncconnector-followup-field-get.md)
- [{#T}](./syncconnector-followup-list.md)
- [{#T}](./syncconnector-followup-get.md)
- [{#T}](./fields.md)
- [{#T}](./index.md)
