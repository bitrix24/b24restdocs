# Get a List of Templates for Sending sign.b2e.template.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`, `crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view e-Signature for HR templates

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The `sign.b2e.template.list` method retrieves the company templates that the user can use to send a document to an employee for signing. The newest templates come first.

The list includes only completed, enabled templates that the user can view. It does not include drafts, hidden templates, or templates for documents that employees send themselves.

To send a document using a template, call the [sign.b2e.template.send](./sign-b2e-template-send.md) method and pass it the template `uid` from the response.

The method works in an application and via an inbound webhook. The application or webhook needs two scopes: `sign.b2e` and `crm`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **pagination**
[`object`](../data-types.md) | Pagination [(detailed description)](#pagination) ||
|#

The method does not accept other parameters, such as `select` or `filter`: a call with them returns a validation error.

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../data-types.md) | Number of templates per page, starting from `1`. Values greater than `50` are reduced to `50`.

Default: `50` ||
|| **page**
[`integer`](../data-types.md) | Page number, starting from `1` ||
|| **offset**
[`integer`](../data-types.md) | Number of templates to skip from the start of the list. If `page` is passed, `offset` is ignored ||
|#

The list has ended when a page contains fewer templates than `limit`. If `limit` is greater than `50`, compare with `50`.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/sign.b2e.template.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"pagination":{"limit":20,"page":1}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/sign.b2e.template.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"pagination":{"limit":20,"page":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/sign.b2e.template.list
    ```

- JS (TS)

    REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TemplateListResult = {
      items: Array<{
        uid: string
        title: string
        companyId: number | null
        companyName: string | null
        regionDocumentType: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateListResult>(
        'sign.b2e.template.list',
        {
          pagination: {
            limit: 20,
            page: 1,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(template => console.info(template.uid, template.title))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listTemplates() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'sign.b2e.template.list',
            {
              pagination: {
                limit: 20,
                page: 1,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(template => console.info(template.uid, template.title))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplates)
    </script>
    ```

- Python

    The SDK sends only the methods from the `Config().api_v3_methods` list to REST 3.0, and the `sign.b2e.*` methods are not in it yet. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "sign.b2e.template.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "pagination": {
                    "limit": 20,
                    "page": 1,
                },
            },
            prefer_version=3,
        )
        for template in response["result"]["items"]:
            print(template["uid"], template["title"])
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

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.template.list',
                [
                    'pagination' => [
                        'limit' => 20,
                        'page' => 1,
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

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'sign.b2e.template.list',
        {
            pagination: {
                limit: 20,
                page: 1,
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

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.template.list',
        [
            'pagination' => [
                'limit' => 20,
                'page' => 1,
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
    res, err := client.Core().Call(ctx, "sign.b2e.template.list", b24.Params{
    	"pagination": b24.Params{
    		"limit": 20,
    		"page":  1,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.template.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		UID                string  `json:"uid"`
    		Title              string  `json:"title"`
    		CompanyID          *int    `json:"companyId"`
    		CompanyName        *string `json:"companyName"`
    		RegionDocumentType *string `json:"regionDocumentType"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, template := range result.Items {
    	fmt.Println(template.UID, template.Title)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "uid": "e2bjlmefhz8tzy1klbdnpnfsk3np08l2",
                "title": "Employment contract",
                "companyId": 2899,
                "companyName": "Müller GmbH",
                "regionDocumentType": "12.999"
            }
        ]
    },
    "time": {
        "start": 1794480659,
        "finish": 1794480659.227879,
        "duration": 0.22787904739379883,
        "processing": 0,
        "date_start": "2026-11-12T11:50:59+01:00",
        "date_finish": "2026-11-12T11:50:59+01:00",
        "operating_reset_at": 1794481200,
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
|| **result.items**
[`array`](../data-types.md) | Array of templates [(detailed description)](#items). If no templates are available, an empty array is returned ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Items Array Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **uid**
[`string`](../data-types.md) | Template identifier. Pass it in the `templateUid` parameter of the [sign.b2e.template.send](./sign-b2e-template-send.md) method ||
|| **title**
[`string`](../data-types.md) | Template name ||
|| **companyId**
[`integer`](../data-types.md) or `null` | Identifier of the CRM company on whose behalf the document is sent. `null` if no company is specified in the template ||
|| **companyName**
[`string`](../data-types.md) or `null` | Name of the company on whose behalf the document is sent. `null` if no company is specified in the template ||
|| **regionDocumentType**
[`string`](../data-types.md) or `null` | Document type code for the region. `null` if the template has no document type ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "SIGN_B2E_ACCESS_DENIED",
        "message": "Insufficient permissions to perform this action in HR documents. Please contact your Bitrix24 administrator."
    }
}
```

{% include notitle [Error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

HTTP status: **403**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_ACCESS_DENIED` | Insufficient permissions to perform this action in HR documents. Please contact your Bitrix24 administrator | Grant the user permission to view e-Signature for HR templates. The method must be called by an active employee: a service account, an extranet user, and a collab guest receive this error ||
|| `SIGN_REST_SCOPE_REQUIRED` | Missing access permission: crm. Add it in the settings of your app or inbound webhook | Add the `crm` scope to the application or webhook ||
|| `SIGN_REST_AUTH_TYPE_NOT_ALLOWED` | This method is available to apps and inbound webhooks only | Call the method from an application or via an inbound webhook ||
|| `SIGN_B2E_UNAVAILABLE` | HR documents are not available in this Bitrix24 | Check that e-Signature for HR is available in your Bitrix24 ||
|| `SIGN_B2E_TEMPLATES_UNAVAILABLE` | Templates for HR documents are not available in this Bitrix24 | Check that e-Signature for HR templates are available in your Bitrix24 ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, and others | Unknown parameter | Pass only `pagination` ||
|| `pagination.limit`, `pagination.page`, `pagination.offset` | An integer is expected | Pass the value as a number ||
|#

#### Pagination Parameter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than `0` ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-template-send.md)
- [{#T}](./sign-b2e-template-folder-tail.md)
- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./index.md)
