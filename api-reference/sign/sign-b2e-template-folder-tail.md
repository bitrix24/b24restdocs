# Get a List of Template Bundles sign.b2e.template.folder.tail

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`, `crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with the "View templates and bundles" permission in e-Signature for HR

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The `sign.b2e.template.folder.tail` method returns the template bundles the user can use to send documents to an employee. A bundle groups several templates of one company.

To retrieve the templates of a bundle, use the [sign.b2e.template.folder.get](./sign-b2e-template-folder-get.md) method. Documents are sent using these templates with the [sign.b2e.template.send](./sign-b2e-template-send.md) method, one call per template.

The method works in an application and via an inbound webhook. The application or webhook needs two scopes: `sign.b2e` and `crm`.

{% note info "" %}

A bundle is not included in the list if:

- the bundle is disabled
- the bundle contains more than 50 templates or refers to a deleted template
- a template in the bundle has no company specified
- the template's company is not a CRM "my company"
- the templates in the bundle belong to different companies
- the user cannot view one of the templates
- one of the templates is disabled, not ready, or hidden, or is a template for a document that the employee sends themselves

A CRM "my company" is your organization on whose behalf documents are sent.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **cursor**
[`object`](../data-types.md) | Pagination cursor [(detailed description)](#cursor). For the first page, omit the parameter or pass `value` equal to `0` ||
|#

The method accepts no other parameters: a call with them returns a validation error.

### Cursor Parameter {#cursor}

Bundles are sorted by `id` in ascending order. The method checks `limit` bundles and returns only those that can be used to send documents. A page may therefore contain fewer bundles than requested, or none at all, even though more bundles remain.

To retrieve all bundles:

1. Call the method without `cursor`
2. If `hasMore` is `true` in the response, pass `cursor` in the next request with `field` equal to `id` and `value` taken from `cursor.value` of the response
3. Repeat until `hasMore` becomes `false`. Only this value means the end of the list

#|
|| **Name**
`type` | **Description** ||
|| **field***
[`string`](../data-types.md) | Cursor field. Only `id` is supported ||
|| **value***
[`integer`](../data-types.md) | Identifier after which the page starts. For the first page — `0`, for the next one — `cursor.value` from the previous response ||
|| **order**
[`string`](../data-types.md) | Traversal direction. Only `ASC` is supported.

Default: `ASC` ||
|| **limit**
[`integer`](../data-types.md) | Number of bundles the method checks per call, starting from `1`. Values greater than `50` are reduced to `50`.

Default: `50` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/sign.b2e.template.folder.tail`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"cursor":{"field":"id","value":0,"order":"ASC","limit":20}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/sign.b2e.template.folder.tail
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"cursor":{"field":"id","value":0,"order":"ASC","limit":20},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/sign.b2e.template.folder.tail
    ```

- JS (TS)

    The `sign.b2e.*` REST 3.0 methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FolderTailResult = {
      cursor: {
        field: string
        value: string | null
      }
      hasMore: boolean
      items: Array<{
        id: number
        title: string
        companyId: number
        companyName: string
        templateCount: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FolderTailResult>(
        'sign.b2e.template.folder.tail',
        {
          cursor: {
            field: 'id',
            value: 0,
            order: 'ASC',
            limit: 20,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(folder => console.info(folder.id, folder.title, folder.templateCount))
        console.info('Next cursor:', result.cursor.value, 'has more:', result.hasMore)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    The `sign.b2e.*` REST 3.0 methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listFolders() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'sign.b2e.template.folder.tail',
            {
              cursor: {
                field: 'id',
                value: 0,
                order: 'ASC',
                limit: 20,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(folder => console.info(folder.id, folder.title, folder.templateCount))
          console.info('Next cursor:', result.cursor.value, 'has more:', result.hasMore)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listFolders)
    </script>
    ```

- Python

    The SDK sends calls to REST 3.0 only for methods in the `Config().api_v3_methods` list, and the `sign.b2e.*` methods are not on it yet. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "sign.b2e.template.folder.tail"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "cursor": {
                    "field": "id",
                    "value": 0,
                    "order": "ASC",
                    "limit": 20,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        for folder in result["items"]:
            print(folder["id"], folder["title"], folder["templateCount"])
        print("Next cursor:", result["cursor"]["value"], "has more:", result["hasMore"])
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.template.folder.tail',
                [
                    'cursor' => [
                        'field' => 'id',
                        'value' => 0,
                        'order' => 'ASC',
                        'limit' => 20,
                    ],
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'sign.b2e.template.folder.tail',
        {
            cursor: {
                field: 'id',
                value: 0,
                order: 'ASC',
                limit: 20,
            },
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.template.folder.tail',
        [
            'cursor' => [
                'field' => 'id',
                'value' => 0,
                'order' => 'ASC',
                'limit' => 20,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.template.folder.tail", b24.Params{
    	"cursor": b24.Params{
    		"field": "id",
    		"value": 0,
    		"order": "ASC",
    		"limit": 20,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.template.folder.tail: %w", err)
    }

    var result struct {
    	Cursor struct {
    		Field string  `json:"field"`
    		Value *string `json:"value"`
    	} `json:"cursor"`
    	HasMore bool `json:"hasMore"`
    	Items   []struct {
    		ID            int    `json:"id"`
    		Title         string `json:"title"`
    		CompanyID     int    `json:"companyId"`
    		CompanyName   string `json:"companyName"`
    		TemplateCount int    `json:"templateCount"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, folder := range result.Items {
    	fmt.Println(folder.ID, folder.Title, folder.TemplateCount)
    }
    fmt.Println("has more:", result.HasMore)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "cursor": {
            "field": "id",
            "value": "1"
        },
        "hasMore": false,
        "items": [
            {
                "id": 1,
                "title": "Onboarding",
                "companyId": 2899,
                "companyName": "Müller GmbH",
                "templateCount": 1
            }
        ]
    },
    "time": {
        "start": 1794480660,
        "finish": 1794480660.234878,
        "duration": 0.2348780632019043,
        "processing": 0,
        "date_start": "2026-11-12T11:51:00+01:00",
        "date_finish": "2026-11-12T11:51:00+01:00",
        "operating_reset_at": 1794481260,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with response data ||
|| **result.cursor**
[`object`](../data-types.md) | Cursor for the next page [(detailed description)](#result-cursor) ||
|| **result.hasMore**
[`boolean`](../data-types.md) | Whether more bundles remain.

Possible values:

- `true` — more bundles remain: request the next page even if this one is empty
- `false` — the list has ended ||
|| **result.items**
[`array`](../data-types.md) | Array of bundles [(detailed description)](#items) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Cursor Object {#result-cursor}

#|
|| **Name**
`type` | **Description** ||
|| **field**
[`string`](../data-types.md) | Cursor field — `id` ||
|| **value**
[`string`](../data-types.md) or `null` | Identifier of the last bundle checked, including a skipped one. Pass it to `cursor.value` of the next request. `null` if the method did not check any bundles ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Bundle identifier. Pass it to the `id` parameter of the [sign.b2e.template.folder.get](./sign-b2e-template-folder-get.md) method ||
|| **title**
[`string`](../data-types.md) | Bundle name ||
|| **companyId**
[`integer`](../data-types.md) | Identifier of the CRM company on whose behalf documents are sent using all templates of the bundle ||
|| **companyName**
[`string`](../data-types.md) | Company name ||
|| **templateCount**
[`integer`](../data-types.md) | Number of templates in the bundle ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The cursor field must be id",
                "field": "cursor.field"
            }
        ]
    }
}
```

{% include notitle [error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

HTTP status: **403**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_ACCESS_DENIED` | Insufficient permissions to perform this action in HR documents. Please contact your Bitrix24 administrator | Grant the user the "View templates and bundles" permission in e-Signature for HR. The method must be called by an active employee: a service account, an extranet user, and a collab guest receive this error ||
|| `SIGN_REST_SCOPE_REQUIRED` | Missing access permission: crm. Add it in the settings of your app or inbound webhook | Add the `crm` scope to the application or webhook ||
|| `SIGN_REST_AUTH_TYPE_NOT_ALLOWED` | This method is available to apps and inbound webhooks only | Call the method from an application or via an inbound webhook ||
|| `SIGN_B2E_UNAVAILABLE` | HR documents are not available in this Bitrix24 | Make sure e-Signature for HR is available in your Bitrix24 ||
|| `SIGN_B2E_TEMPLATES_UNAVAILABLE` | Templates for HR documents are not available in this Bitrix24 | Make sure e-Signature for HR templates are available in your Bitrix24 ||
|| `SIGN_B2E_TEMPLATE_FOLDERS_UNAVAILABLE` | Template bundles are not available in this Bitrix24 | Send documents using individual templates from [sign.b2e.template.list](./sign-b2e-template-list.md) ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `cursor.field` | The cursor field must be id | Pass `field` equal to `id` ||
|| `cursor.value` | A cursor value of 0 or greater is expected | Pass `0` or `cursor.value` from the previous response ||
|| `cursor.order` | Only the ASC cursor order is supported | Pass `ASC` or omit `order` ||
|| `cursor.value`, `cursor.limit` | An integer is expected | Pass the value as a number or a string of digits ||
|| parameter name | Unknown parameter | Remove the parameter that is not in the description ||
|#

#### Pagination Parameter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `cursor.limit` | Incorrect "pagination" parameter value: `cursor.limit` | Pass `limit` greater than `0` ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-template-folder-get.md)
- [{#T}](./sign-b2e-template-send.md)
- [{#T}](./sign-b2e-template-list.md)
- [{#T}](./index.md)
