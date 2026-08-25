# Create a Sales Intelligence Trace: crm.tracking.trace.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user can create a trace
> - a user with permission to modify the object can link the trace

The method `crm.tracking.trace.add` creates a Sales Intelligence trace and returns its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TRACE**^*^
[`string`](../../data-types.md) | JSON string containing trace data.

You can retrieve a ready-to-use value on the website via `b24Tracker.guest.getTrace()`. A practical scenario is described in the [tutorial](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md).

If you create `TRACE` manually, pass UTM parameters in the `tags.list` object:

```json
{"tags":{"list":{"utm_source":"google-maps"}}}
```

In this example, only one UTM parameter is passed to the trace: `utm_source` with the value `google-maps`. To pass other UTM parameters, add them to `tags.list`, for example `utm_medium`, `utm_campaign`, `utm_content`, or `utm_term` ||
|| **ENTITIES**
[`object[]`](../../data-types.md) | Array of objects to be linked with the trace [(detailed description)](#entities)

If `ENTITIES` is not passed, the method creates a trace without linking it to CRM objects ||
|#

### Parameter ENTITIES {#entities}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**^*^
[`string`](../../data-types.md) | CRM object type.

Possible values:

- `COMPANY` — company, `entityTypeId = 4`
- `CONTACT` — contact, `entityTypeId = 3`
- `DEAL` — deal, `entityTypeId = 2`
- `LEAD` — lead, `entityTypeId = 1`
- `QUOTE` — estimate, `entityTypeId = 7`
||
|| **ID**^*^
[`integer`](../../data-types.md) | CRM object identifier.

You can retrieve the identifier using the [crm.item.list](../universal/crm-item-list.md) method with `entityTypeId` of the required object type.

The user must have modification rights for the specified object ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of creating a Sales Intelligence trace, where:
- `TRACE` — JSON string with the `utm_source` UTM parameter and the value `google-maps`
- `ENTITIES` — contact and lead linked to the trace

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "TRACE": "{\"tags\":{\"list\":{\"utm_source\":\"google-maps\"}}}",
        "ENTITIES": [
          {
            "TYPE": "CONTACT",
            "ID": 17
          },
          {
            "TYPE": "LEAD",
            "ID": 1000739
          }
        ]
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/crm.tracking.trace.add.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "TRACE": "{\"tags\":{\"list\":{\"utm_source\":\"google-maps\"}}}",
        "ENTITIES": [
          {
            "TYPE": "CONTACT",
            "ID": 17
          },
          {
            "TYPE": "LEAD",
            "ID": 1000739
          }
        ],
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/crm.tracking.trace.add.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.tracking.trace.add',
        params: {
          TRACE: '{"tags":{"list":{"utm_source":"google-maps"}}}',
          ENTITIES: [
            {
              TYPE: 'CONTACT',
              ID: 17,
            },
            {
              TYPE: 'LEAD',
              ID: 1000739,
            },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created trace ID:', result)
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
      async function addTrackingTrace() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.tracking.trace.add',
            params: {
              TRACE: '{"tags":{"list":{"utm_source":"google-maps"}}}',
              ENTITIES: [
                {
                  TYPE: 'CONTACT',
                  ID: 17,
                },
                {
                  TYPE: 'LEAD',
                  ID: 1000739,
                },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created trace ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTrackingTrace)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.tracking.trace.add(
            trace="{\"tags\":{\"list\":{\"utm_source\":\"google-maps\"}}}",
            entities=[
                {
                    "TYPE": "CONTACT",
                    "ID": 17
                },
                {
                    "TYPE": "LEAD",
                    "ID": 1000739
                }
            ],
        )
        result = bitrix_response.response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.tracking.trace.add',
                [
                    'TRACE' => '{"tags":{"list":{"utm_source":"google-maps"}}}',
                    'ENTITIES' => [
                        [
                            'TYPE' => 'CONTACT',
                            'ID' => 17,
                        ],
                        [
                            'TYPE' => 'LEAD',
                            'ID' => 1000739,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding trace: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.tracking.trace.add',
        {
            TRACE: '{"tags":{"list":{"utm_source":"google-maps"}}}',
            ENTITIES: [
                {
                    TYPE: 'CONTACT',
                    ID: 17
                },
                {
                    TYPE: 'LEAD',
                    ID: 1000739
                }
            ]
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'crm.tracking.trace.add',
        [
            'TRACE' => '{"tags":{"list":{"utm_source":"google-maps"}}}',
            'ENTITIES' => [
                [
                    'TYPE' => 'CONTACT',
                    'ID' => 17,
                ],
                [
                    'TYPE' => 'LEAD',
                    'ID' => 1000739,
                ],
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.tracking.trace.add", b24.Params{
        "TRACE": "{\"tags\":{\"list\":{\"utm_source\":\"google-maps\"}}}",
    	"ENTITIES": []b24.Params{
    		{
    			"TYPE": "CONTACT",
                "ID":   17,
    		},
    		{
    			"TYPE": "LEAD",
                "ID":   1000739,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.tracking.trace.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 1581,
    "time": {
        "start": 1786711683,
        "finish": 1786711683.296598,
        "duration": 0.296597957611084,
        "processing": 0,
        "date_start": "2026-08-14T15:48:03+03:00",
        "date_finish": "2026-08-14T15:48:03+03:00",
        "operating_reset_at": 1786712283,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created trace ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Parameter `TRACE` required."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Parameter `TRACE` required. | The `TRACE` parameter is not provided ||
|| `400` | `ERROR_CORE` | Can not parse JSON in parameter `TRACE`. | The value `TRACE` is not a valid JSON string ||
|| `400` | `ERROR_CORE` | Wrong TYPE in parameter `ENTITIES`. Allowed types: COMPANY,CONTACT,DEAL,LEAD,QUOTE | An invalid `TYPE` was passed in `ENTITIES` ||
|| `400` | `ERROR_CORE` | Wrong ID in parameter `ENTITIES`. | An incorrect `ID` was passed in `ENTITIES` ||
|| `400` | `ERROR_CORE` | You have no access to entity `<TYPE>` with ID `<ID>`. | No permission to modify the object specified in `ENTITIES` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-lead.md)
- [{#T}](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-contact.md)
