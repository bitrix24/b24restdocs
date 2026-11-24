# Get CRM Items of a Task Template v 3.0 tasks.template.crm.item.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.crm.item.list` method returns CRM items linked to a task template.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../data-types.md) | Task template ID.

The ID can be retrieved with the [tasks.template.add](./tasks-template-add-rest-v3.md) method when creating a template or with the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|| **select**
[`string[]`](../../data-types.md) | CRM item fields to return.

Possible values:

- `id` — item ID
- `type` — item type
- `title` — item title

If the parameter is not passed, the method returns all three fields ||
|#

The method returns only the items the user can read in CRM. The list is returned in full: the method does not support filtering, sorting, or pagination. If the CRM module is not installed, the list is empty.

Template links to CRM are set in the `crmItemIds` field with the [tasks.template.add](./tasks-template-add-rest-v3.md) and [tasks.template.update](./tasks-template-update-rest-v3.md) methods. Full item codes like `D_9193` are returned by the [tasks.template.get](./tasks-template-get-rest-v3.md) method in the `crmItemIds` field.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.crm.item.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":125,"select":["id","type","title"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.crm.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":125,"select":["id","type","title"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.crm.item.list
    ```

- JS (TS)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateCrmItemListResult = {
      items: Array<{
        id: number
        type: 'deal' | 'lead' | 'contact' | 'company' | 'unknown'
        title: string
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCrmItemListResult>(
        'tasks.template.crm.item.list',
        {
          templateId: 125,
          select: ['id', 'type', 'title'],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listTemplateCrmItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.crm.item.list',
            {
              templateId: 125,
              select: ['id', 'type', 'title'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplateCrmItems)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.crm.item.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 125,
                "select": ["id", "type", "title"],
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
                'tasks.template.crm.item.list',
                [
                    'templateId' => 125,
                    'select' => ['id', 'type', 'title'],
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
        'tasks.template.crm.item.list',
        {
            templateId: 125,
            select: ['id', 'type', 'title'],
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
        'tasks.template.crm.item.list',
        [
            'templateId' => 125,
            'select' => ['id', 'type', 'title'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.crm.item.list", b24.Params{
    	"templateId": 125,
    	"select":     []string{"id", "type", "title"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.crm.item.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("the response has no items key")
    }

    var items []struct {
    	ID    int    `json:"id"`
    	Type  string `json:"type"`
    	Title string `json:"title"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(len(items))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 9193,
                "type": "deal",
                "title": "Deal #6531"
            },
            {
                "id": 4575,
                "type": "contact",
                "title": "Equipment supply"
            },
            {
                "id": 2985,
                "type": "company",
                "title": "Vektor GmbH"
            },
            {
                "id": 1001345,
                "type": "lead",
                "title": "Complete CRM form \"Docs check v7\""
            },
            {
                "id": 117,
                "type": "unknown",
                "title": "Invoice for equipment payment"
            }
        ]
    },
    "time": {
        "start": 1795429063,
        "finish": 1795429063.161092,
        "duration": 0.16109204292297363,
        "processing": 0,
        "date_start": "2026-11-23T11:17:43+01:00",
        "date_finish": "2026-11-23T11:17:43+01:00",
        "operating_reset_at": 1795429620,
        "operating": 0.28139710426330566
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
[`object[]`](../../data-types.md) | Array of CRM items linked to the template [(detailed description)](#items). If there are no links or the user cannot read the linked items, the array is empty ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object items {#items}

The object contains only the fields listed in `select`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | CRM item ID without a type prefix, for example, `9193` for the deal `D_9193` ||
|| **type**
[`string`](../../data-types.md) | CRM item type.

Possible values:

- `deal` — deal
- `lead` — lead
- `contact` — contact
- `company` — company
- `unknown` — another type, for example, an invoice or an SPA item. To find out the type of such an item, compare `id` with the codes in the `crmItemIds` field of the [tasks.template.get](./tasks-template-get-rest-v3.md) response ||
|| **title**
[`string`](../../data-types.md) | CRM item title ||
|#

## Error Handling

HTTP status: **400**, **403**, **404**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION",
        "message": "Record with ID \"999999\" was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the template ID as a number ||
|#

#### Unknown Field in select

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `CrmItemDto` | Keep only `id`, `type`, and `title` in `select` ||
|#

#### Template Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | Check the template ID with the [tasks.template.list](./tasks-template-list-rest-v3.md) method. The method does not find a deleted template either ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permission to view the template with the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) method and make sure the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-update-rest-v3.md)
- [{#T}](./tasks-template-access-get-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
