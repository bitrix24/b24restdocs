# Add Child Templates v 3.0 tasks.template.relation.child.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the parent template and permission to edit each template being linked as a child

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `tasks.template.relation.child.add` links templates to a parent template as child templates.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Identifier of the parent template to which the child templates are linked.

The template identifier can be obtained when [creating a template](../tasks-template-add-rest-v3.md) or by using the [template list](../tasks-template-list-rest-v3.md) method ||
|| **templateIds***
[`integer[]`](../../../data-types.md) | Array of identifiers of the templates to make child templates. Pass from 1 to 20 unique identifiers; the method ignores duplicates ||
|| **noOverride**
[`boolean`](../../../data-types.md) | Prohibits relinking templates that already have a parent template.

Possible values:

- `true` — if one of the templates from `templateIds` already has a parent template, the method returns an error and links nothing
- `false` — a template with a different parent template is moved to the template from `templateId`

Default: `false` ||
|#

{% note info "" %}

Either all templates from `templateIds` are linked, or none are. If at least one template is not found, is unavailable, or linking would create a cycle, the method returns an error and does not change the relations.

{% endnote %}

{% note warning "" %}

A template cannot be linked to itself or to its own child template at any nesting level: such a link would create a cycle.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.relation.child.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":117,"templateIds":[119,121],"noOverride":true}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.relation.child.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":117,"templateIds":[119,121],"noOverride":true,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.relation.child.add
    ```

- JS (TS)

    The `tasks.template.relation.*` methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ChildAddResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ChildAddResult>(
        'tasks.template.relation.child.add',
        {
          templateId: 117,
          templateIds: [119, 121],
          noOverride: true,
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Child templates attached:', result.result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    The `tasks.template.relation.*` methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addChildTemplates() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.relation.child.add',
            {
              templateId: 117,
              templateIds: [119, 121],
              noOverride: true,
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Child templates attached:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addChildTemplates)
    </script>
    ```

- Python

    The `tasks.template.relation.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `bitrix_token.call_method`, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.relation.child.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 117,
                "templateIds": [119, 121],
                "noOverride": True,
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
                'tasks.template.relation.child.add',
                [
                    'templateId' => 117,
                    'templateIds' => [119, 121],
                    'noOverride' => true,
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
        'tasks.template.relation.child.add',
        {
            templateId: 117,
            templateIds: [119, 121],
            noOverride: true,
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
        'tasks.template.relation.child.add',
        [
            'templateId' => 117,
            'templateIds' => [119, 121],
            'noOverride' => true,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.relation.child.add", b24.Params{
    	"templateId":  117,
    	"templateIds": []int{119, 121},
    	"noOverride":  true,
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.relation.child.add: %w", err)
    }

    var out struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1795429010,
        "finish": 1795429010.41892,
        "duration": 0.41892004013061523,
        "processing": 0,
        "date_start": "2026-11-23T11:16:50+01:00",
        "date_finish": "2026-11-23T11:16:50+01:00",
        "operating_reset_at": 1795429560,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response.

Contains an object with the `result` key set to `true` if the templates are linked ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_RELATIONCONFLICTEXCEPTION",
        "message": "Cannot link the template as a child: it already has a parent template or linking it would create a cycle."
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the identifier as a number, not a string ||
|| `templateIds` | Required field `templateIds` is missing | Pass a non-empty array of template identifiers ||
|| `templateIds` | This request requires data type `integer` in field `templateIds` | Pass the identifiers as numbers, not strings ||
|| `templateIds` | This request requires data type `array` in field `templateIds` | Pass an array, not a single number. If the error is returned for an array, check its size: a single request accepts no more than 20 unique identifiers ||
|#

#### Template Not Found

HTTP status: **404**

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId`, `templateIds` | Record with ID `999999` was not found | Check the identifiers using the [tasks.template.get](../tasks-template-get-rest-v3.md) method. The method treats a deleted template as not found ||
|#

#### Relation Conflict

HTTP status: **409**

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_RELATIONCONFLICTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateIds` | Cannot link the template as a child: it already has a parent template or linking it would create a cycle | If `noOverride: true` is passed, check with the [tasks.template.relation.child.ids](./tasks-template-relation-child-ids-rest-v3.md) method whether the templates have a parent, and unlink them with the [tasks.template.relation.child.delete](./tasks-template-relation-child-delete-rest-v3.md) method or pass `noOverride: false`. If the error is caused by a cycle, remove the parent template and its parents at any level from `templateIds` ||
|#

#### Access Error

HTTP status: **403**

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to read the template from `templateId` and permission to edit each template from `templateIds` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-relation-child-delete-rest-v3.md)
- [{#T}](./tasks-template-relation-child-ids-rest-v3.md)
- [{#T}](./tasks-template-relation-child-list-rest-v3.md)
- [{#T}](./tasks-template-relation-child-list-by-ids-rest-v3.md)
- [{#T}](../fields-rest-v3.md)
