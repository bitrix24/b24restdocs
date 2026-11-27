# Get Bundle Templates sign.b2e.template.folder.get

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

The `sign.b2e.template.folder.get` method returns a template bundle: its name, company, and the templates themselves. The newest templates come first, and their fields are the same as in the response of the [sign.b2e.template.list](./sign-b2e-template-list.md) method.

The method finds only a bundle that can be used to send documents. These are the same bundles that [sign.b2e.template.folder.tail](./sign-b2e-template-folder-tail.md) returns; the selection rules are described on its page. Documents are sent using the bundle templates with the [sign.b2e.template.send](./sign-b2e-template-send.md) method, one call per template.

The method works in an application and via an inbound webhook. The application or webhook needs two scopes: `sign.b2e` and `crm`.

{% note warning "" %}

The contents of a bundle may change, so retrieve it right before sending. Each document of the bundle is sent with a separate `sign.b2e.template.send` call. If one of the calls returns an error, such as `SIGN_B2E_MEMBER_ROLE_NOT_RESOLVED` or `SIGN_B2E_TEMPLATE_NOT_FOUND`, the bundle has not been sent in full: fix the cause and send the missing documents.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Bundle identifier. You can retrieve it with the [sign.b2e.template.folder.tail](./sign-b2e-template-folder-tail.md) method ||
|#

The method accepts no other parameters, such as `select`: a call with them returns a validation error.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/sign.b2e.template.folder.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/sign.b2e.template.folder.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/sign.b2e.template.folder.get
    ```

- JS (TS)

    The `sign.b2e.*` REST 3.0 methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FolderGetResult = {
      item: {
        id: number
        title: string
        companyId: number
        companyName: string
        templateCount: number
        templates: Array<{
          uid: string
          title: string
          companyId: number | null
          companyName: string | null
          regionDocumentType: string | null
        }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FolderGetResult>(
        'sign.b2e.template.folder.get',
        {
          id: 1,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.item.templates.forEach(template => console.info(template.uid, template.title))
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
      async function getFolder() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'sign.b2e.template.folder.get',
            {
              id: 1,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.item.templates.forEach(template => console.info(template.uid, template.title))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFolder)
    </script>
    ```

- Python

    The SDK sends calls to REST 3.0 only for methods in the `Config().api_v3_methods` list, and the `sign.b2e.*` methods are not on it yet. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "sign.b2e.template.folder.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 1,
            },
            prefer_version=3,
        )
        for template in response["result"]["item"]["templates"]:
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.template.folder.get',
                [
                    'id' => 1,
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
        'sign.b2e.template.folder.get',
        {
            id: 1,
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
        'sign.b2e.template.folder.get',
        [
            'id' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.template.folder.get", b24.Params{
    	"id": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.template.folder.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID            int    `json:"id"`
    		Title         string `json:"title"`
    		TemplateCount int    `json:"templateCount"`
    		Templates     []struct {
    			UID   string `json:"uid"`
    			Title string `json:"title"`
    		} `json:"templates"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, template := range result.Item.Templates {
    	fmt.Println(template.UID, template.Title)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 1,
            "title": "Onboarding",
            "companyId": 2899,
            "companyName": "Müller GmbH",
            "templateCount": 1,
            "templates": [
                {
                    "uid": "e2bjlmefhz8tzy1klbdnpnfsk3np08l2",
                    "title": "Employment contract",
                    "companyId": 2899,
                    "companyName": "Müller GmbH",
                    "regionDocumentType": "12.999"
                }
            ]
        }
    },
    "time": {
        "start": 1794480660,
        "finish": 1794480660.685563,
        "duration": 0.6855630874633789,
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
|| **result.item**
[`object`](../data-types.md) | Bundle [(detailed description)](#item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Bundle identifier ||
|| **title**
[`string`](../data-types.md) | Bundle name ||
|| **companyId**
[`integer`](../data-types.md) | Identifier of the CRM company on whose behalf documents are sent using all templates of the bundle ||
|| **companyName**
[`string`](../data-types.md) | Company name ||
|| **templateCount**
[`integer`](../data-types.md) | Number of templates in the bundle ||
|| **templates**
[`array`](../data-types.md) | Bundle templates. The template fields are described on the [sign.b2e.template.list](./sign-b2e-template-list.md#items) page. Pass the template `uid` to the `templateUid` parameter of the [sign.b2e.template.send](./sign-b2e-template-send.md) method ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "SIGN_B2E_TEMPLATE_FOLDER_NOT_FOUND",
        "message": "Bundle not found or unavailable for sending. Select one of the available bundles.",
        "validation": [
            {
                "field": "id",
                "code": "SIGN_B2E_TEMPLATE_FOLDER_NOT_FOUND"
            }
        ]
    }
}
```

{% include notitle [error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Bundle Not Found

HTTP status: **404**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_TEMPLATE_FOLDER_NOT_FOUND` | Bundle not found or unavailable for sending. Select one of the available bundles | Take the `id` from the [sign.b2e.template.folder.tail](./sign-b2e-template-folder-tail.md) response. The same error is returned if the bundle does not exist, the user cannot view it, or the bundle cannot be used to send documents ||
|#

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
|| `id` | Required field `id` is missing | Add `id` to the request body ||
|| `id` | Field value must be 1 or greater | Pass a bundle identifier greater than `0` ||
|| `id` | An integer is expected | Pass the identifier as a number ||
|| parameter name | Unknown parameter | Remove the parameter that is not in the description ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-template-folder-tail.md)
- [{#T}](./sign-b2e-template-send.md)
- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./index.md)
