# Add Comment crm.timeline.comment.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to edit the CRM item the comment is added to

The `crm.timeline.comment.add` method adds a comment to the timeline of a CRM item.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a comment. Request body example:

```json
{
    "fields": {
        "ENTITY_ID": 10,
        "ENTITY_TYPE": "deal",
        "COMMENT": "New comment",
        "FILES": [
            [
                "1.gif",
                "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="
            ]
        ]
    }
}
```

`FILES` takes an array of files. Each file is an array of the file name and the content as a [base64 string](../../../files/how-to-upload-files.md). If no files are passed, the comment is created without them

{% note warning %}

Starting from CRM version 23.100.0, the method accepts the `fields` parameter only with its name in lowercase. Other undocumented variants (`Fields`, `FIELDS`, `arFields`) are not accepted.

{% endnote %}

 ||
|#

### Parameter fields {#parametr-fields}

The full field description is in the [`crm.timeline.comment.fields`](./crm-timeline-comment-fields.md#fields) method. Use the following fields to add a comment.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Positive identifier of the CRM item the comment is added to.

The value can be obtained using the method [`crm.item.list`](../../universal/crm-item-list.md) or when creating an element with the help of [`crm.item.add`](../../universal/crm-item-add.md) ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Symbolic name of the CRM item type, for example `deal` or `dynamic_1046`. Type examples are in the [field description](./crm-timeline-comment-fields.md#fields).

Required if a non-zero `ENTITY_TYPE_ID` is not passed ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | [Numeric identifier of the CRM object type](../../data-types.md#object_type), for example `2` for a deal. If passed and not equal to `0`, it is used instead of `ENTITY_TYPE` ||
|| **COMMENT***
[`string`](../../../data-types.md) | The text of the comment. Must not be empty after leading and trailing spaces, tabs, and line breaks are removed. The current user becomes the author ||
|| **FILES**
[`attached_diskfile`](../../data-types.md#attached_diskfile) | Optional array of files in the format shown in the request body example above ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":10,"ENTITY_TYPE":"deal","COMMENT":"New comment was added","FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":10,"ENTITY_TYPE":"deal","COMMENT":"New comment was added","FILES":[["1.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="],["2.gif","R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="]]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.add
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
        method: 'crm.timeline.comment.add',
        params: {
          fields: {
            ENTITY_ID: 10,
            ENTITY_TYPE: 'deal',
            COMMENT: 'New comment was added',
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
        console.info('Added comment ID:', result)
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
      async function addTimelineComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.comment.add',
            params: {
              fields: {
                ENTITY_ID: 10,
                ENTITY_TYPE: 'deal',
                COMMENT: 'New comment was added',
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
          console.info('Added comment ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTimelineComment)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.comment.add(
            fields={
                "ENTITY_ID": 10,
                "ENTITY_TYPE": "deal",
                "COMMENT": "New comment was added",
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
                'crm.timeline.comment.add',
                [
                    'fields' => [
                        'ENTITY_ID'   => 10,
                        'ENTITY_TYPE' => 'deal',
                        'COMMENT'     => 'New comment was added',
                        'FILES'       => [
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
        echo 'Error adding timeline comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.comment.add",
        {
            fields:
            {
                "ENTITY_ID": 10,
                "ENTITY_TYPE": "deal",
                "COMMENT": "New comment was added",
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
        }, result => {
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
        'crm.timeline.comment.add',
        [
            'fields' => [
                'ENTITY_ID' => 10,
                'ENTITY_TYPE' => 'deal',
                'COMMENT' => 'New comment was added',
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
    res, err := client.Core().Call(ctx, "crm.timeline.comment.add", b24.Params{
    	"fields": b24.Params{
    		"ENTITY_ID":   10,
    		"ENTITY_TYPE": "deal",
    		"COMMENT":     "New comment was added",
    		"FILES": []any{
    			[]string{"1.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="},
    			[]string{"2.gif", "R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw=="},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.timeline.comment.add: %w", err)
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
[`integer`](../../../data-types.md) | Returns the integer identifier of the added comment ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "INVALID_ARG_VALUE",
    "error_description": "Empty comment message"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `OWNER_NOT_FOUND` | Owner was not found | `ENTITY_ID` is not passed or is not positive, or the CRM item type could not be determined from `ENTITY_TYPE_ID` or `ENTITY_TYPE` ||
|| `400` | `ACCESS_DENIED` | Access denied | No permission to edit the CRM item ||
|| `400` | `INVALID_ARG_VALUE` | Empty comment message | `COMMENT` is not passed or is empty after leading and trailing spaces, tabs, and line breaks are removed ||
|| `400` | `ADDING_DISABLED` | Could not create comment | The comment could not be created ||
|| `400` | `100` | Could not find value for parameter {fields} | The required `fields` object is not passed ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-update.md)
- [{#T}](./crm-timeline-comment-get.md)
- [{#T}](./crm-timeline-comment-list.md)
- [{#T}](./crm-timeline-comment-delete.md)
- [{#T}](./crm-timeline-comment-fields.md)
- [{#T}](./events/on-Crm-Timeline-Comment-Add.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-comment-to-spa.md)