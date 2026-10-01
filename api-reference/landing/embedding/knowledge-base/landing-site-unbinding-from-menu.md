# Unbind Knowledge Base from Menu landing.site.unbindingFromMenu

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View" permission in the "Sites" area and the "Add to Extensions menu" permission in the "Knowledge Base" section

The method `landing.site.unbindingFromMenu` unbinds the Knowledge Base from the specified menu. Bindings of this Knowledge Base to other menus and the Knowledge Base itself are retained.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the Knowledge Base site.

`id` can be retrieved:
- from the `ENTITY_ID` field of the [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) method
- from the `ID` field of the [landing.site.getList](../../site/landing-site-get-list.md#type-scope) method with the `scope: "KNOWLEDGE"` parameter — without it, `landing.site.getList` does not return Knowledge Bases ||
|| **menuCode**^*^
[`string`](../../../data-types.md) | Menu code.

`menuCode` can be obtained:
- in the interface through the "Select Knowledge Base" option: in the URL of the opened frame, the `menuId` parameter contains the menu code, for example `menuId=crm_switcher:deal`
- from the result of the method [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) in the `BINDING_ID` field ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of unbinding the Knowledge Base from the menu, where:
- `id` — identifier of the Knowledge Base site
- `menuCode` — menu code

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 31,
        "menuCode": "crm_switcher:deal"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.unbindingFromMenu.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 31,
        "menuCode": "crm_switcher:deal",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.unbindingFromMenu.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.site.unbindingFromMenu',
        params: {
          id: 31,
          menuCode: 'crm_switcher:deal',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result)
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
      async function unbindSiteFromMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.unbindingFromMenu',
            params: {
              id: 31,
              menuCode: 'crm_switcher:deal',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unbindSiteFromMenu)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.landing.site.unbinding_from_menu(
            bitrix_id=31,
            menu_code="crm_switcher:deal",
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
                'landing.site.unbindingFromMenu',
                [
                    'id' => 31,
                    'menuCode' => 'crm_switcher:deal',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unbinding site from menu: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.unbindingFromMenu',
        {
            id: 31,
            menuCode: 'crm_switcher:deal'
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
    require_once('crest.php');

    $result = CRest::call(
        'landing.site.unbindingFromMenu',
        [
            'id' => 31,
            'menuCode' => 'crm_switcher:deal',
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.site.unbindingFromMenu", b24.Params{
    	"id":       31,
    	"menuCode": "crm_switcher:deal",
    })
    if err != nil {
    	return fmt.Errorf("landing.site.unbindingFromMenu: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774959544,
        "finish": 1774959544.732957,
        "duration": 0.7329568862915039,
        "processing": 0,
        "date_start": "2026-03-31T15:19:04+02:00",
        "date_finish": "2026-03-31T15:19:04+02:00",
        "operating_reset_at": 1774960144,
        "operating": 0
    }
}
```

If the binding is not removed:

```json
{
    "result": false,
    "time": {
        "start": 1774952712,
        "finish": 1774952712.384215,
        "duration": 0.38421511650085449,
        "processing": 0,
        "date_start": "2026-03-31T13:25:12+03:00",
        "date_finish": "2026-03-31T13:25:12+03:00",
        "operating_reset_at": 1774953312,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the unbinding:

- `true` — binding removed
- `false` — binding not removed

The method returns `false` without an error if:
- no Knowledge Base site with the specified `id` is found, or the user does not have permission to view it
- the user does not have the "Add to Extensions menu" permission in the "Knowledge Base" section
- this Knowledge Base is not bound to the `menuCode` menu

A `false` response does not distinguish between these cases. You can check the current bindings with the [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Some of the call parameters were missing: menuCode"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MISSING_PARAMS` | Some of the call parameters were missing: menuCode | Method call without `menuCode` ||
|| `400` | `MISSING_PARAMS` | Some of the call parameters were missing: id | Method call without `id` ||
|| `400` | `TYPE_ERROR` | — | A non-numeric value, such as `abc`, is passed in `id` ||
|| `400` | `TYPE_ERROR` | Invalid type of the call argument: id | The `id` parameter is passed as an array ||
|| `400` | `TYPE_ERROR` | Invalid type of the call argument: menuCode | The `menuCode` parameter is passed as an array ||
|| `400` | `ACCESS_DENIED` | Insufficient permission. | The method is called by an extranet user, or the user does not have the "View" permission in the "Sites" area ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-binding-to-menu.md)
- [{#T}](./landing-site-get-menu-bindings.md)
- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./landing-site-unbinding-from-group.md)
- [{#T}](./index.md)