# Get a Card View v 3.0 crm.[type].detailpagelayout.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user. An additional card view is available to members of its audience and to a user with permission to edit the common card view. With the `userId` parameter of another user, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.detailpagelayout.get` method returns a CRM item card view by ID: its sections and the fields in them, and for an additional view, also its name and audience. The object type is set by the `<type>` segment of the method name.

If the personal or common view has not been configured yet, the method returns the default view.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.detailpagelayout.get` ||
|| Lead | `crm.lead.detailpagelayout.get` ||
|| Contact | `crm.contact.detailpagelayout.get` ||
|| Company | `crm.company.detailpagelayout.get` ||
|| Estimate | `crm.quote.detailpagelayout.get` ||
|| Invoice | `crm.smartinvoice.detailpagelayout.get` ||
|| CRM document | `crm.smartdocument.detailpagelayout.get` ||
|| B2E document | `crm.smartb2edocument.detailpagelayout.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.detailpagelayout.get`, for example, `crm.smartprocess1038.detailpagelayout.get` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. SPAs have card view methods regardless of the type settings. My company does not have these methods.

The method name is written in lowercase: `crm.deal.detailpagelayout.get`, not `crm.deal.detailPageLayout.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Card view ID, a string:

- `C<categoryId>:personal` — personal view, for example, `C69:personal`
- `C<categoryId>:common` — common view, for example, `C69:common`
- a number in a string, for example, `"17"` — additional view

Leads and estimates have no pipelines, so their personal and common views are `personal` and `common`. For repeat leads, a lead has separate views: `personal_returning_customer` and `common_returning_customer`.

The view must belong to the pipeline from `categoryId`. Personal and common view IDs are returned by the [crm.&lt;type&gt;.detailpagelayout.getpersonallayoutid](./crm-entity-detail-page-layout-get-personal-layout-id-rest-v3.md) and [crm.&lt;type&gt;.detailpagelayout.getcommonlayoutid](./crm-entity-detail-page-layout-get-common-layout-id-rest-v3.md) methods, and additional view IDs are returned by the [crm.&lt;type&gt;.detailpagelayout.list](./crm-entity-detail-page-layout-list-rest-v3.md) method ||
|| **categoryId***
[`integer`](../../../data-types.md) | Pipeline ID. Card views are stored separately for each pipeline.

The value depends on the object type:

- lead and estimate — only `0`, they have no pipelines
- deal, contact, and company — pipeline ID, `0` is the default pipeline
- invoice, documents, and SPA — pipeline ID. The method replaces `0` with the ID of the default pipeline

Pipeline IDs of deals, contacts, companies, and SPAs are returned by the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method ||
|| **userId**
[`integer`](../../../data-types.md) | ID of the user whose personal view to return or on whose behalf to check access to an additional view. Only a CRM administrator can pass the ID of another user.

Defaults to the user who calls the method.

User IDs are returned by the [user.get](../../../user/user-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.detailpagelayout.get`

{% endnote %}

The examples retrieve the additional view `17` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"id":"17"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.detailpagelayout.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"id":"17","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.detailpagelayout.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type LayoutElement = {
      name: string
      optionFlags?: number
      options?: Record<string, unknown>
    }

    type LayoutSection = {
      name: string
      title: string
      elements: LayoutElement[]
    }

    type Layout = {
      id: string
      configuration: LayoutSection[]
      name: string | null
      audience: { type: string; id: number }[] | null
    }

    type LayoutGetResult = {
      item: Layout
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<LayoutGetResult>(
        'crm.smartprocess1038.detailpagelayout.get',
        { categoryId: 0, id: '17' },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.id, result.item.configuration)
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
      async function getLayout() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.detailpagelayout.get',
            { categoryId: 0, id: '17' },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.id, result.item.configuration)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getLayout)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.detailpagelayout.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={"categoryId": 0, "id": "17"},
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"])
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
                'crm.smartprocess1038.detailpagelayout.get',
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
        'crm.smartprocess1038.detailpagelayout.get',
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
        'crm.smartprocess1038.detailpagelayout.get',
        ['categoryId' => 0, 'id' => '17']
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.detailpagelayout.get", b24.Params{"categoryId": 0, "id": "17"}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.detailpagelayout.get: %w", err)
    }

    var layout struct {
    	ID            string  `json:"id"`
    	Name          *string `json:"name"`
    	Configuration []struct {
    		Name     string `json:"name"`
    		Title    string `json:"title"`
    		Elements []struct {
    			Name        string         `json:"name"`
    			OptionFlags *int           `json:"optionFlags"`
    			Options     map[string]any `json:"options"`
    		} `json:"elements"`
    	} `json:"configuration"`
    	Audience []struct {
    		Type string `json:"type"`
    		ID   int    `json:"id"`
    	} `json:"audience"`
    }
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("response has no item")
    }
    if err := json.Unmarshal(raw, &layout); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(layout.ID, len(layout.Configuration))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": "17",
            "configuration": [
                {
                    "name": "main",
                    "title": "About the request",
                    "elements": [
                        {
                            "name": "TITLE"
                        },
                        {
                            "name": "STAGE_ID"
                        },
                        {
                            "name": "OPPORTUNITY_WITH_CURRENCY",
                            "optionFlags": 1
                        }
                    ]
                },
                {
                    "name": "additional",
                    "title": "Additional",
                    "elements": [
                        {
                            "name": "ASSIGNED_BY_ID"
                        }
                    ]
                }
            ],
            "name": "Procurement and warehouse card",
            "audience": [
                {
                    "type": "department",
                    "id": 47
                },
                {
                    "type": "user",
                    "id": 29
                }
            ]
        }
    },
    "time": {
        "start": 1793706656,
        "finish": 1793706656.382836,
        "duration": 0.38283610343933105,
        "processing": 0,
        "date_start": "2026-11-03T14:50:56+03:00",
        "date_finish": "2026-11-03T14:50:56+03:00",
        "operating_reset_at": 1793707200,
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
|| **result.item**
[`object`](../../../data-types.md) | Card view [(detailed description)](#layout) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Card View Object {#layout}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Card view ID: `C69:personal` and `C69:common` are the personal and common views of the pipeline; for leads and estimates, `personal` and `common`. A number in a string, for example, `"17"`, is an additional view ||
|| **configuration**
[`object[]`](../../../data-types.md) | Card sections in display order [(detailed description)](#section) ||
|| **name**
[`string`](../../../data-types.md) | Name of the additional view. For personal and common views, `null` ||
|| **audience**
[`object[]`](../../../data-types.md) | Members who have access to the additional view [(detailed description)](#audience). For personal and common views, `null` ||
|#

#### Card Section {#section}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Section code, for example, `main` ||
|| **title**
[`string`](../../../data-types.md) | Section title in the card ||
|| **elements**
[`object[]`](../../../data-types.md) | Section fields in display order. Each field has `name` — the field name in the card, for example, `TITLE`. The `optionFlags` and `options` keys are present only if they are saved in the view: `optionFlags` is `1` if the field is always shown, and `0` if it is shown only when filled in. `options` contains field settings, for example, `{"defaultCountry": "DE"}` ||
|#

#### Audience Member {#audience}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Member type: `user` — employee, `department` — department with its subdepartments, `project` — workgroup or project, `structure-node` — company structure node ||
|| **id**
[`integer`](../../../data-types.md) | Member ID ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "LAYOUT_NOT_FOUND",
        "message": "The form layout was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

Card view errors are returned with short codes without the `BITRIX_REST_V3_` prefix, for example, `LAYOUT_NOT_FOUND`, and with a message in English. The HTTP status of these errors varies and is given next to the code. Errors with `BITRIX_REST_V3_*` codes are returned with status 400.

#### Card View Not Found

Error Code: `LAYOUT_NOT_FOUND`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The form layout was not found | Check `id`: the additional view does not exist, belongs to a different pipeline, or the user has no access to it. The list of available views is returned by the [crm.&lt;type&gt;.detailpagelayout.list](./crm-entity-detail-page-layout-list-rest-v3.md) method ||
|#

#### Invalid Card View ID

Error Code: `INVALID_LAYOUT_ID`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout ID | Check the `id` format and the pipeline. For example, `C37:common` with the `categoryId` of a different pipeline causes this error ||
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
|| `-` | Insufficient permissions to work with this form layout | Only a CRM administrator can retrieve another user's personal view ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.detailpagelayout.get`. My company has no card view methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-detail-page-layout-add-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-update-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-list-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-delete-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-get-current-rest-v3.md)
