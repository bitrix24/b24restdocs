# Get a Set of Additional Content Blocks of the activity crm.activity.layout.blocks.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to read the CRM entity to which the activity is linked

The method `crm.activity.layout.blocks.get` retrieves the set of additional content blocks of an activity.

The method only works in the context of the [application](../../../../../settings/app-installation/index.md): when called via a webhook, it returns the `ERROR_WRONG_CONTEXT` error. The application sees only the set of blocks that it installed itself using the [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) method.

The method works only with activities. To retrieve the set of blocks of a comment or another timeline entry, use [crm.timeline.layout.blocks.get](../../layout-blocks/crm-timeline-layout-blocks-get.md).

If the activity is linked to several CRM entities at once, the set of blocks remains a single one. Pass the type and the identifier of any of the linked entities in `entityTypeId` and `entityId`.

The order in which the methods are called and the general rules for working with block sets are described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***  
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the activity is linked, for example `2` for a deal ||
|| **entityId***  
[`integer`](../../../../data-types.md) | Identifier of the CRM object to which the activity is linked, for example the deal identifier ||
|| **activityId***  
[`integer`](../../../../data-types.md) | Identifier of the activity. It is returned by the [crm.activity.add](../activity-base/crm-activity-add.md) and [crm.activity.list](../activity-base/crm-activity-list.md) methods ||
|#

## Code Examples

Get the set of additional content blocks of the activity with `id = 8`, linked to the deal with `id = 4`.

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.layout.blocks.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type LayoutBlocksGetResult = {
      // layout is null when the app has not set any blocks for this activity
      layout: null | {
        blocks: Record<string, {
          type: string
          properties: Record<string, unknown>
        }>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<LayoutBlocksGetResult>({
        method: 'crm.activity.layout.blocks.get',
        params: {
          entityTypeId: 2,
          entityId: 4,
          activityId: 8,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Layout blocks:', result.layout?.blocks)
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
      async function getLayoutBlocks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.layout.blocks.get',
            params: {
              entityTypeId: 2,
              entityId: 4,
              activityId: 8,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          // layout is null when the app has not set any blocks for this activity
          console.info('Layout blocks:', result.layout?.blocks)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getLayoutBlocks)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.layout.blocks.get(
            entity_type_id=2,
            entity_id=4,
            activity_id=8,
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
                'crm.activity.layout.blocks.get',
                [
                    'entityTypeId' => 2,
                    'entityId'     => 4,
                    'activityId'   => 8,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Info: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting activity layout blocks: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.activity.layout.blocks.get',
        {
            entityTypeId: 2, // Deal
            entityId: 4,     // Deal ID
            activityId: 8,   // ID of the activity linked to this deal
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');
    $result = CRest::call(
        'crm.activity.layout.blocks.get',
        [
            'entityTypeId' => 2,
            'entityId' => 4,
            'activityId' => 8
        ]
    );
    echo '';
    print_r($result);
    echo '';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.layout.blocks.get", b24.Params{
    	"entityTypeId": 2,
    	"entityId":     4,
    	"activityId":   8,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.layout.blocks.get: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "layout": {
            "blocks": {
                "block_1": {
                    "type": "text",
                    "properties": {
                        "value": "Hello!\nWe are starting.",
                        "multiline": true,
                        "bold": true,
                        "color": "base_90"
                    }
                },
                "block_2": {
                    "type": "largeText",
                    "properties": {
                        "value": "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."
                    }
                },
                "block_3": {
                    "type": "link",
                    "properties": {
                        "text": "Open deal",
                        "bold": true,
                        "action": {
                            "type": "redirect",
                            "uri": "/crm/deal/details/123/"
                        }
                    }
                },
                "block_4": {
                    "type": "withTitle",
                    "properties": {
                        "title": "Title",
                        "block": {
                            "type": "text",
                            "properties": {
                                "value": "Some value"
                            }
                        }
                    }
                }
            }
        }
    },
    "time": {
        "start": 1753341040.475739,
        "finish": 1753341040.582705,
        "duration": 0.10696601867675781,
        "processing": 0.04708504676818848,
        "date_start": "2025-07-24T17:57:20+00:00",
        "date_finish": "2025-07-24T17:57:20+00:00",
        "operating": 0
    }
}
```

If no set of blocks is installed:

```json
{
    "result": {
        "layout": null
    },
    "time": {
        "start": 1753341040.475739,
        "finish": 1753341040.582705,
        "duration": 0.10696601867675781,
        "processing": 0.04708504676818848,
        "date_start": "2025-07-24T17:57:20+00:00",
        "date_finish": "2025-07-24T17:57:20+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response [(detailed description)](#result). If the set of blocks could not be retrieved, the method returns an `error` object instead of `result` — see the "Error Handling" section ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **layout**
[`RestAppLayoutDto`](../configurable/structure/rest-app-layout-dto.md) | The set of additional content blocks installed in the activity by the current application [(detailed description)](#layout). If the application has not installed a set of blocks in this activity, the field has the value `null` ||
|#

#### Object layout {#layout}

#|
|| **Name**
`type` | **Description** ||
|| **blocks**
[`object`](../../../../data-types.md) | Associative array of [content blocks](../configurable/structure/content-block.md) exactly as the application passed them to [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md). The key is the block identifier set by the application. The value is an object with the `type` and `properties` fields ||
|#

The `type` field takes one of the values `text`, `largeText`, `link`, `deadline`, `withTitle`, `lineOfBlocks`. The composition of `properties` depends on the block type and is described in the [ContentBlockDto](../configurable/structure/content-block.md) structure.

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "The method can only be called in the context of a rest application"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a rest application. The method was called via a webhook ||
|| `OWNER_NOT_FOUND` | The element to which the activity is linked was not found. An unknown `entityTypeId` was passed, or the activity is not linked to the element with the specified `entityId` ||
|| `NOT_FOUND` | The activity with the specified `activityId` was not found ||
|| `ACCESS_DENIED` | The user has no permission to read the CRM entity to which the activity is linked ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-layout-blocks-set.md)
- [{#T}](./crm-activity-layout-blocks-delete.md)
- [{#T}](../configurable/structure/content-block.md)
- [{#T}](../../layout-blocks/content-blocks-test-app.md)