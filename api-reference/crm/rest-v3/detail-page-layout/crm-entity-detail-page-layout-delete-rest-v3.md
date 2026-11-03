# Delete an Additional Card View v 3.0 crm.[type].detailpagelayout.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the common card view of this object type, for example, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.detailpagelayout.delete` method deletes an additional card view of a CRM item. The object type is set by the `<type>` segment of the method name.

Only an additional view can be deleted. The personal and common views cannot be deleted: for their IDs, the method returns the `INVALID_LAYOUT_ID` error.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.detailpagelayout.delete` ||
|| Lead | `crm.lead.detailpagelayout.delete` ||
|| Contact | `crm.contact.detailpagelayout.delete` ||
|| Company | `crm.company.detailpagelayout.delete` ||
|| Estimate | `crm.quote.detailpagelayout.delete` ||
|| Invoice | `crm.smartinvoice.detailpagelayout.delete` ||
|| CRM document | `crm.smartdocument.detailpagelayout.delete` ||
|| B2E document | `crm.smartb2edocument.detailpagelayout.delete` ||
|| SPA | `crm.smartprocess<entityTypeId>.detailpagelayout.delete`, for example, `crm.smartprocess1038.detailpagelayout.delete` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. SPAs have card view methods regardless of the type settings. My company does not have these methods.

The method name is written in lowercase: `crm.deal.detailpagelayout.delete`, not `crm.deal.detailPageLayout.delete`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Additional card view ID — a number in a string, for example, `"17"`. The personal and common views cannot be deleted.

Additional view IDs are returned by the [crm.&lt;type&gt;.detailpagelayout.list](./crm-entity-detail-page-layout-list-rest-v3.md) method ||
|| **categoryId***
[`integer`](../../../data-types.md) | Pipeline ID. Card views are stored separately for each pipeline.

The value depends on the object type:

- lead and estimate — only `0`, they have no pipelines
- deal, contact, and company — pipeline ID, `0` is the default pipeline
- invoice, documents, and SPA — pipeline ID. The method replaces `0` with the ID of the default pipeline

Pipeline IDs of deals, contacts, companies, and SPAs are returned by the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method ||
|| **userId**
[`integer`](../../../data-types.md) | ID of the user on whose behalf to check access to the view. Only a CRM administrator can pass the ID of another user.

Defaults to the user who calls the method.

User IDs are returned by the [user.get](../../../user/user-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.detailpagelayout.delete`

{% endnote %}

The examples delete the additional view `17` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"id":"17"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.detailpagelayout.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"id":"17","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.detailpagelayout.delete
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type LayoutChangeResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<LayoutChangeResult>(
        'crm.smartprocess1038.detailpagelayout.delete',
        { categoryId: 0, id: '17' },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Layout deleted:', result.result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteLayout() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.detailpagelayout.delete',
            { categoryId: 0, id: '17' },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Layout deleted:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteLayout)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.detailpagelayout.delete"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={"categoryId": 0, "id": "17"},
            prefer_version=3,
        )
        result = response["result"]
        print(result["result"])
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.detailpagelayout.delete',
                ['categoryId' => 0, 'id' => '17']
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.detailpagelayout.delete',
        { categoryId: 0, id: '17' },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.detailpagelayout.delete',
        ['categoryId' => 0, 'id' => '17']
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.detailpagelayout.delete", b24.Params{"categoryId": 0, "id": "17"})
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.detailpagelayout.delete: %w", err)
    }

    var answer struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &answer); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(answer.Result)
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
        "start": 1793706822,
        "finish": 1793706822.893266,
        "duration": 0.893265962600708,
        "processing": 0,
        "date_start": "2026-11-03T14:53:42+03:00",
        "date_finish": "2026-11-03T14:53:42+03:00",
        "operating_reset_at": 1793707380,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the view is deleted ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "INVALID_LAYOUT_ID",
        "message": "Invalid form layout ID."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

Card view errors are returned with short codes without the `BITRIX_REST_V3_` prefix, for example, `LAYOUT_NOT_FOUND`, and with a message in English. The HTTP status of these errors varies and is given next to the code. Errors with `BITRIX_REST_V3_*` codes are returned with status 400.

#### Invalid Card View ID

Error Code: `INVALID_LAYOUT_ID`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout ID | Pass the ID of an additional view — a number in a string. The personal and common views cannot be deleted ||
|#

#### Card View Not Found

Error Code: `LAYOUT_NOT_FOUND`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The form layout was not found | Check `id`: the view does not exist, has already been deleted, belongs to a different pipeline, or the user has no permission to delete it ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `categoryId` | Required field `categoryId` is missing | Pass `categoryId`. For leads and estimates, pass `0` ||
|| `categoryId` | This request requires data type `int` in field `categoryId` | Pass `categoryId` as a number, not a string ||
|| `categoryId` | Field value must be 0 or greater | Pass `0` or a pipeline ID ||
|| `userId` | Field value must be 1 or greater | Pass the ID of an existing user or omit `userId` ||
|| `id` | Required field `id` is missing | Pass the view ID ||
|| `id` | This request requires data type `string` in field `id` | Pass `id` as a string, for example, `"17"` ||
|#

#### Unknown Parameter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `LayoutDto` | Remove the parameter from the request. The method accepts only `id`, `categoryId`, and `userId` ||
|#

#### Unsupported Pipeline

Error Code: `UNSUPPORTED_CONTEXT`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | This form layout context is not supported | Check `categoryId`: for leads and estimates, pass `0`; for other types, pass the ID of an existing pipeline of this object type ||
|#

#### Access Denied

Error Code: `PERMISSION_DENIED`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient permissions to work with this form layout | Only a CRM administrator can pass the `userId` parameter with another user ||
|#

#### Storage Error

Error Code: `STORAGE_FAILED`, HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Could not complete the operation on the form layout | Retry the request later. If the error persists, contact support ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.detailpagelayout.delete`. My company has no card view methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-detail-page-layout-add-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-update-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-get-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-list-rest-v3.md)
