# Delete a set of additional content blocks from the CRM activity crm.activity.layout.blocks.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to modify the CRM entity to which the activity is linked

The method `crm.activity.layout.blocks.delete` removes a set of additional content blocks from an activity.

The method only works in the context of the [application](../../../../../settings/app-installation/index.md): when called via a webhook, it returns the `ERROR_WRONG_CONTEXT` error. The application removes only the set of blocks that it installed itself using the [crm.activity.layout.blocks.set](./crm-activity-layout-blocks-set.md) method.

The method works only with activities. To remove the set of blocks of a comment or another timeline entry, use [crm.timeline.layout.blocks.delete](../../layout-blocks/crm-timeline-layout-blocks-delete.md).

The method is not idempotent: calling it again for an already deleted set returns the `NOT_FOUND` error.

If the activity is linked to several CRM entities at once, the blocks stop being displayed in the timeline of every linked entity.

When an application is deleted, all block sets it added are removed automatically — there is no need to call this method beforehand.

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

Delete a set of additional content blocks from the activity with `id = 8`, linked to the deal with `id = 4`.

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.layout.blocks.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DeleteBlocksResult = {
      success: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<DeleteBlocksResult>({
        method: 'crm.activity.layout.blocks.delete',
        params: {
          entityTypeId: 2, // Deal
          entityId: 4,     // Deal ID
          activityId: 8,   // Activity ID linked to this deal
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.success)
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
      async function deleteActivityLayoutBlocks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.layout.blocks.delete',
            params: {
              entityTypeId: 2, // Deal
              entityId: 4,     // Deal ID
              activityId: 8,   // Activity ID linked to this deal
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.success)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteActivityLayoutBlocks)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.layout.blocks.delete(
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
                'crm.activity.layout.blocks.delete',
                [
                    'entityTypeId' => 2, // Deal
                    'entityId'     => 4, // Deal ID
                    'activityId'   => 8, // ID of the deal linked to this deal
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting activity layout block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.activity.layout.blocks.delete',
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
        'crm.activity.layout.blocks.delete',
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
    res, err := client.Core().Call(ctx, "crm.activity.layout.blocks.delete", b24.Params{
    	"entityTypeId": 2,
    	"entityId":     4,
    	"activityId":   8,
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.layout.blocks.delete: %w", err)
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
        "success": true
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
[`object`](../../../../data-types.md) | Root element of the response [(detailed description)](#result). If the set of blocks could not be deleted, the method returns an `error` object instead of `result` — see the "Error Handling" section ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **success**
[`boolean`](../../../../data-types.md) | Result of deleting the set of additional content blocks. The field is returned when the method completes successfully and has the value `true` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "The method can only be called in the context of a REST application"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a REST application. The method was called via a webhook ||
|| `OWNER_NOT_FOUND` | The element to which the activity is linked was not found. An unknown `entityTypeId` was passed, or the activity is not linked to the element with the specified `entityId` ||
|| `NOT_FOUND` | The activity was not found, or the application did not install a set of blocks in it ||
|| `ACCESS_DENIED` | The user has no permission to modify the CRM entity to which the activity is linked ||
|#

The code and the text of the `NOT_FOUND` error are the same in both situations. To tell them apart, call [crm.activity.layout.blocks.get](./crm-activity-layout-blocks-get.md): for a non-existent activity it also returns `NOT_FOUND`, while for an activity without a set of blocks it returns `layout` with the value `null`.

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-layout-blocks-set.md)
- [{#T}](./crm-activity-layout-blocks-get.md)
- [{#T}](../configurable/structure/content-block.md)
- [{#T}](../../layout-blocks/content-blocks-test-app.md)