# Activate the crm.automation.trigger

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission to modify the target object `target`

The `crm.automation.trigger` method launches a CRM automation webhook trigger for an object. The "Track Inbound Webhook" trigger is configured in CRM automation, and Bitrix24 provides a ready-made URL for it of the form

```bash
https://mydomain.bitrix24.com/rest/1/not_var{{PASSWORD}}/crm.automation.trigger/?target=DEAL_not_var{{ID}}&code=nwly5
```

A call to this URL from an external source passes the call to CRM automation. If a matching trigger fires, the CRM object moves to the stage the trigger is bound to.

The URL works through a [local inbound webhook](../../../local-integrations/local-webhooks.md) that Bitrix24 creates for the trigger.

The method can be called via an inbound webhook and from [local](../../../local-integrations/local-apps.md) and [mass-market](../../../market/index.md) applications. An application's own triggers, registered with the [crm.automation.trigger.add](./triggers/crm-automation-trigger-add.md) method, are launched with the [crm.automation.trigger.execute](./triggers/crm-automation-trigger-execute.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **target***
[`string`](../../data-types.md) | Target object for automation in the form of [`TYPENAME_ID`](../data-types.md#object_type), for example, `LEAD_25` or `DEAL_57`. For an SPA item, use `DYNAMIC_<entityTypeId>_<ID>`, for example, `DYNAMIC_174_1` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the trigger configured in CRM automation for a specific stage or status. The value is taken from the `code` parameter in the trigger URL in its settings. A trigger with a code matches the call only if `code` matches. A trigger without a code matches any `code` value or no value at all ||
|#

{% note info %}

In rare cases, multiple triggers may be detected for the specified `target` object. This happens if:

- `code` is not passed in the request and there are old triggers in Bitrix24 that do not have a `code`
- a `code` is passed that turns out to be the same for multiple triggers
- a `code` is passed while, besides the trigger with this `code`, a trigger without a code is configured for the object

In this case, the first matching trigger in stage order fires, provided its conditions are met and the transition to its stage is allowed.

Whether a trigger fires also depends on its "Conditions" and the "Allow transitioning to the previous status" option.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"target":"DEAL_57","code":"c5u4m"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automation.trigger
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"target":"DEAL_57","code":"c5u4m","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger
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
        method: 'crm.automation.trigger',
        params: {
          target: 'DEAL_57',
          code: 'c5u4m',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Request accepted:', result)
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
      async function triggerAutomation() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.automation.trigger',
            params: {
              target: 'DEAL_57',
              code: 'c5u4m',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Request accepted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', triggerAutomation)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.automation.trigger(
            target="DEAL_57",
            code="c5u4m",
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
                'crm.automation.trigger',
                [
                    'target' => 'DEAL_57',
                    'code'   => 'c5u4m',
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

    ```js
    BX24.callMethod(
        "crm.automation.trigger",
        {
            target: 'DEAL_57',
            code: 'c5u4m',
        },
        function(result) 
        {
            if(result.error())
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
        'crm.automation.trigger',
        [
            'target' => 'DEAL_57',
            'code' => 'c5u4m'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.automation.trigger", b24.Params{
    	"target": "DEAL_57",
    	"code":   "c5u4m",
    })
    if err != nil {
    	return fmt.Errorf("crm.automation.trigger: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("request accepted:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start":1718809827.810153,
        "finish":1718809828.541046,
        "duration":0.7308928966522217,
        "processing":0.09834408760070801,
        "date_start":"2024-06-19T15:10:27+00:00",
        "date_finish":"2024-06-19T15:10:28+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` means the request has been accepted. The value does not confirm that the object has changed its stage: `true` is returned, for example, if there is no trigger with this `code` or no object with this `ID`. To check the transition, use the object's stage field, for example, with the [crm.&lt;type&gt;.get](../rest-v3/items/crm-entity-get-rest-v3.md) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"",
    "error_description":"Target is not set."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Target is not set. | The required parameter `target` was not provided ||
|| `400` | Empty value | Incorrect target format. | `target` has no `_` separator between the type and the ID. The required format is `TYPENAME_ID` ||
|| `400` | Empty value | Target is not found. | `target` contains an unknown `TYPENAME` type or an ID that equals zero after conversion to an integer, for example, `DEAL_abc` or `DEAL_0` ||
|| `403` | `ACCESS_DENIED` | Access denied! There is no permissions to update the entity. | The user does not have permission to modify the object from `target` ||
|| `400` | Empty value | Access denied. | The user does not have access to CRM ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./triggers/index.md)
- [{#T}](../rest-v3/items/crm-entity-get-rest-v3.md)
