# Update Comment crm.timeline.comment.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: the comment author with permission to edit the linked CRM item

The `crm.timeline.comment.update` method updates a comment in the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Integer identifier of the comment (e.g., `1`). Identifiers can be obtained using the [`crm.timeline.comment.list`](./crm-timeline-comment-list.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for updating the comment. Request body example:

```json
{
    "id": 999,
    "fields": {
        "COMMENT": "Updated comment",
        "FILES": [
            [
                "1.gif",
                "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="
            ]
        ]
    }
}
```

{% note warning %}

Starting from CRM version 23.100.0, the method accepts the `fields` parameter only with its name in lowercase. Other undocumented variants (`Fields`, `FIELDS`, `arFields`) are not accepted.

{% endnote %}

||
|| **ownerTypeId**
[`integer`](../../../data-types.md) | [Integer identifier of the CRM object type](../../data-types.md#object_type) to which the comment is linked (e.g., `2` for a deal) ||
|| **ownerId**
[`integer`](../../../data-types.md) | Integer identifier of the CRM object to which the comment is attached (e.g., `1`). You can obtain a list of identifiers using the [`crm.timeline.bindings.list`](../bindings/crm-timeline-bindings-list.md) method (field `ENTITY_ID`) ||
|#


Pass `ownerTypeId` and `ownerId` together. If at least one of the parameters is not passed, the method determines both from the comment bindings and selects the first binding for which the user has permission to edit the CRM item. If there is no such binding, the method returns the `ACCESS_DENIED` error.

{% note warning %}

If a comment is bound to multiple CRM items, the method does not change the original comment. It creates a new comment with the same author and creation date, binds it only to the selected item, and removes the binding to this item from the original comment. The other items keep the original text. The method returns the identifier of the new comment, and handlers receive the [onCrmTimelineCommentAdd](./events/on-Crm-Timeline-Comment-Add.md) and [onCrmTimelineCommentUpdate](./events/on-Crm-Timeline-Comment-Update.md) events with this identifier.

{% endnote %}

### Parameter fields {#parametr-fields}

The full field reference is available in the [`crm.timeline.comment.fields`](./crm-timeline-comment-fields.md) method description. Consider the following requirements when updating.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMENT***
[`string`](../../../data-types.md) | The text of the comment. Must not be empty after leading and trailing spaces, tabs, and line breaks are removed. Pass it on every update, including when changing files ||
|| **FILES**
[`attached_diskfile`](../../data-types.md#attached_diskfile) | List of files. An array of values described according to the [rules](../../../files/how-to-update-files.md). When only `COMMENT` is updated, the current files are retained ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"COMMENT":"Comment was changed","FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"fields":{"COMMENT":"Comment was changed","FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.update
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
        method: 'crm.timeline.comment.update',
        params: {
          id: 999,
          fields: {
            COMMENT: 'Comment was changed',
            FILES: [
              ['1.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
              ['2.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated comment id:', result)
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
      async function updateTimelineComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.comment.update',
            params: {
              id: 999,
              fields: {
                COMMENT: 'Comment was changed',
                FILES: [
                  ['1.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
                  ['2.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
                ],
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated comment id:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTimelineComment)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.comment.update(
            bitrix_id=999,
            fields={
                "COMMENT": "Comment was changed",
                "FILES": [
                    ["1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],
                    ["2.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],
                ],
            },
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
                'crm.timeline.comment.update',
                [
                    'id'     => 999,
                    'fields' => [
                        'COMMENT' => 'Comment was changed',
                        'FILES'  => [
                            ['1.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
                            ['2.gif', 'R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=='],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating timeline comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.comment.update",
        {
            id: 999,
            fields:
            {
                "COMMENT": "Comment was changed",
                "FILES": [
                    [
                        "1.gif", 
                        "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="
                    ],
                    [
                        "2.gif",
                        "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="
                    ],
                ]
            }
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

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.comment.update',
        [
            'id' => 999,
            'fields' => [
                'COMMENT' => 'Comment was changed',
                'FILES' => [
                    ["1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],
                    ["2.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.timeline.comment.update", b24.Params{
    	"id": 999,
    	"fields": b24.Params{
    		"COMMENT": "Comment was changed",
    		"FILES": []any{
    			[]string{"1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="},
    			[]string{"2.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.timeline.comment.update: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 999,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-07T17:19:01+03:00",
        "date_finish": "2024-05-07T17:19:01+03:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the updated comment. If the comment was bound to multiple CRM items, the identifier of the new comment is returned ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item was not found."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `OWNER_NOT_FOUND` | Owner was not found | `ownerId` is less than or equal to zero, or an unknown `ownerTypeId` is passed ||
|| `400` | `ACCESS_DENIED` | Access denied | The user is not the comment author or has no permission to edit the linked CRM item ||
|| `400` | `NOT_FOUND` | Item was not found | The comment was not found, has no bindings, or the specified CRM item is not linked to the comment ||
|| `400` | `ADDING_DISABLED` | Could not create comment | The comment is bound to multiple CRM items, and a new comment for the selected item could not be created ||
|| `400` | `INVALID_ARG_VALUE` | Empty comment message | The comment text is not passed or is empty after leading and trailing spaces, tabs, and line breaks are removed ||
|| `400` | `100` | Could not find value for parameter {fields} | The required `fields` parameter is not passed ||
|| `400` | `100` | Could not find value for parameter {id} | The required `id` parameter is not passed ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-add.md)
- [{#T}](./crm-timeline-comment-get.md)
- [{#T}](./crm-timeline-comment-list.md)
- [{#T}](./crm-timeline-comment-delete.md)
- [{#T}](./crm-timeline-comment-fields.md)