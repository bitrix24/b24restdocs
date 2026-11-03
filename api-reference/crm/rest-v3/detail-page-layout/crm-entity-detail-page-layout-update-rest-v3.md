# Update a Card View v 3.0 crm.[type].detailpagelayout.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user — their own personal view; the common and additional views — a user with permission to edit the common card view of this object type, for example, a CRM administrator; another user's personal view — a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.detailpagelayout.update` method updates a personal, common, or additional card view for CRM items. The object type is set by the `<type>` segment of the method name.

For the personal and common views, only the set of sections and fields `configuration` can be changed. For an additional view, you can also change the name `name` and the audience `audience`. The method changes only the `fields` keys that are passed, and each passed key replaces the whole value: a new `configuration` array replaces all sections, and a new `audience` array replaces all members.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.detailpagelayout.update` ||
|| Lead | `crm.lead.detailpagelayout.update` ||
|| Contact | `crm.contact.detailpagelayout.update` ||
|| Company | `crm.company.detailpagelayout.update` ||
|| Estimate | `crm.quote.detailpagelayout.update` ||
|| Invoice | `crm.smartinvoice.detailpagelayout.update` ||
|| CRM document | `crm.smartdocument.detailpagelayout.update` ||
|| B2E document | `crm.smartb2edocument.detailpagelayout.update` ||
|| SPA | `crm.smartprocess<entityTypeId>.detailpagelayout.update`, for example, `crm.smartprocess1038.detailpagelayout.update` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. Type IDs can be retrieved with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. Card view methods are available for an SPA regardless of the type settings. My company has no such methods.

The method name is written in lowercase: `crm.deal.detailpagelayout.update`, not `crm.deal.detailPageLayout.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Card view ID, a string:

- `C<categoryId>:personal` — the personal view, for example, `C69:personal`
- `C<categoryId>:common` — the common view, for example, `C69:common`
- a number in a string, for example, `"17"` — an additional view

Leads and estimates have no pipelines, so their personal and common views are `personal` and `common`. Leads also have separate views for repeat leads: `personal_returning_customer` and `common_returning_customer`.

The view must belong to the pipeline from `categoryId`. IDs of the personal and common views are returned by the [crm.&lt;type&gt;.detailpagelayout.getpersonallayoutid](./crm-entity-detail-page-layout-get-personal-layout-id-rest-v3.md) and [crm.&lt;type&gt;.detailpagelayout.getcommonlayoutid](./crm-entity-detail-page-layout-get-common-layout-id-rest-v3.md) methods, and IDs of additional views by the [crm.&lt;type&gt;.detailpagelayout.list](./crm-entity-detail-page-layout-list-rest-v3.md) method ||
|| **categoryId***
[`integer`](../../../data-types.md) | Pipeline ID. Card views are stored separately for each pipeline.

The value depends on the object type:

- lead and estimate — only `0`, as they have no pipelines
- deal, contact, and company — the pipeline ID; `0` is the default pipeline
- invoice, documents, and SPA — the pipeline ID. The method replaces the value `0` with the ID of the default pipeline

Pipeline IDs of deals, contacts, companies, and SPAs can be retrieved with the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method ||
|| **userId**
[`integer`](../../../data-types.md) | ID of the user whose view to update. Required to update another user's personal view. Only a CRM administrator can pass the ID of another user.

Defaults to the user who calls the method.

User IDs can be retrieved with the [user.get](../../../user/user-get.md) method ||
|| **fields***
[`object`](../../../data-types.md) | New view values [(detailed description)](#fields) ||
|#

### The fields Parameter {#fields}

Pass at least one key.

#|
|| **Name**
`type` | **Description** ||
|| **configuration**
[`object[]`](../../../data-types.md) | Card sections in display order [(detailed description)](#configuration). The array replaces all sections of the view, so pass the whole configuration. The current configuration is returned by the [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) method ||
|| **name**
[`string`](../../../data-types.md) | New name of the additional view, up to 100 characters. The personal and common views have no name ||
|| **audience**
[`object[]`](../../../data-types.md) | New list of members of the additional view, at least one [(detailed description)](#audience). The list replaces the previous one entirely. The user who calls the method is not added to the audience automatically. The personal and common views have no audience ||
|#

### The configuration Parameter {#configuration}

An array of card sections in display order. Each section is an object:

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Section code, unique within the view, for example, `main` ||
|| **title***
[`string`](../../../data-types.md) | Section title in the card ||
|| **elements***
[`object[]`](../../../data-types.md) | Section fields in display order [(detailed description)](#elements). You can pass an empty array ||
|#

#### Section Field {#elements}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field name in the card: `TITLE`, `STAGE_ID`, `OPPORTUNITY_WITH_CURRENCY`, custom field `UF_CRM_1687529546`. The names are written in uppercase and do not match the item field names in REST 3.0 ||
|| **optionFlags**
[`integer`](../../../data-types.md) | Whether to show the field when it is empty.

Possible values:

- `1` — always show
- `0` — show only if the field is filled in ||
|| **options**
[`object`](../../../data-types.md) | Field settings in the format `{"setting": "value"}`, for example, `{"defaultCountry": "DE"}` for a phone number. The set of settings depends on the field. Field settings are available in the view configuration returned by the [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) method ||
|#

The method does not check whether the object type has a field with this name: a view with an unknown field name is saved. Take ready-made field names and their settings from the common view configuration, which is returned by the [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) method with the ID from [crm.&lt;type&gt;.detailpagelayout.getcommonlayoutid](./crm-entity-detail-page-layout-get-common-layout-id-rest-v3.md).

### The audience Parameter {#audience}

An array of members who can access the additional view. Each member is an object:

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../../data-types.md) | Member type.

Possible values:

- `user` — an employee. The ID can be retrieved with the [user.get](../../../user/user-get.md) method
- `department` — a department together with its subdepartments. The ID can be retrieved with the [department.get](../../../departments/department-get.md) method
- `project` — a group or project. The ID can be retrieved with the [sonet_group.get](../../../sonet-group/sonet-group-get.md) method
- `structure-node` — a company structure node together with its child nodes ||
|| **id***
[`integer`](../../../data-types.md) | Member ID, a positive number ||
|#

The audience can have up to 1000 members.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.detailpagelayout.update`

{% endnote %}

The examples change the name and audience of the additional view `17` of an SPA with the type ID `1038`: the view becomes available to department `47` and employee `29`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"id":"17","fields":{"name":"Procurement and warehouse card","audience":[{"type":"department","id":47},{"type":"user","id":29}]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.detailpagelayout.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"id":"17","fields":{"name":"Procurement and warehouse card","audience":[{"type":"department","id":47},{"type":"user","id":29}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.detailpagelayout.update
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
        'crm.smartprocess1038.detailpagelayout.update',
        {
          categoryId: 0,
          id: '17',
          fields: {
            name: 'Procurement and warehouse card',
            audience: [
              { type: 'department', id: 47 },
              { type: 'user', id: 29 },
            ],
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Layout updated:', result.result)
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
      async function updateLayout() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.detailpagelayout.update',
            {
              categoryId: 0,
              id: '17',
              fields: {
                name: 'Procurement and warehouse card',
                audience: [
                  { type: 'department', id: 47 },
                  { type: 'user', id: 29 },
                ],
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Layout updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateLayout)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.detailpagelayout.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "categoryId": 0,
                "id": "17",
                "fields": {
                    "name": "Procurement and warehouse card",
                    "audience": [
                        {"type": "department", "id": 47},
                        {"type": "user", "id": 29},
                    ],
                },
            },
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
                'crm.smartprocess1038.detailpagelayout.update',
                [
                    'categoryId' => 0,
                    'id' => '17',
                    'fields' => [
                        'name' => 'Procurement and warehouse card',
                        'audience' => [
                            ['type' => 'department', 'id' => 47],
                            ['type' => 'user', 'id' => 29],
                        ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.detailpagelayout.update',
        {
            categoryId: 0,
            id: '17',
            fields: {
                name: 'Procurement and warehouse card',
                audience: [
                    { type: 'department', id: 47 },
                    { type: 'user', id: 29 },
                ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.detailpagelayout.update',
        [
            'categoryId' => 0,
            'id' => '17',
            'fields' => [
                'name' => 'Procurement and warehouse card',
                'audience' => [
                    ['type' => 'department', 'id' => 47],
                    ['type' => 'user', 'id' => 29],
                ],
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.detailpagelayout.update", b24.Params{
    	"categoryId": 0,
    	"id":         "17",
    	"fields": b24.Params{
    		"name": "Procurement and warehouse card",
    		"audience": []b24.Params{
    			{"type": "department", "id": 47},
    			{"type": "user", "id": 29},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.detailpagelayout.update: %w", err)
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
        "start": 1793706651,
        "finish": 1793706651.905107,
        "duration": 0.9051070213317871,
        "processing": 0,
        "date_start": "2026-11-03T14:50:51+03:00",
        "date_finish": "2026-11-03T14:50:51+03:00",
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
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the view was updated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "INVALID_LAYOUT_AUDIENCE",
        "message": "Invalid form layout audience."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

Card view errors are returned with short codes without the `BITRIX_REST_V3_` prefix, for example, `LAYOUT_NOT_FOUND`, and with English messages. The HTTP status of these errors varies and is given next to the code. Errors with `BITRIX_REST_V3_*` codes are returned with status 400.

#### Invalid View Configuration

Error Code: `INVALID_LAYOUT_CONFIGURATION`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout configuration | Do not pass `name` for the personal and common views. Check that `name` is no longer than 100 characters and that `configuration` is not an empty array ||
|#

#### Invalid View Audience

Error Code: `INVALID_LAYOUT_AUDIENCE`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout audience | Do not pass `audience` for the personal and common views. Check that `audience` is not an empty array and has no more than 1000 members ||
|#

#### Invalid Card View ID

Error Code: `INVALID_LAYOUT_ID`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout ID | Check the `id` format and the pipeline: the view ID must belong to the pipeline from `categoryId` ||
|#

#### Card View Not Found

Error Code: `LAYOUT_NOT_FOUND`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The form layout was not found | Check `id`: the additional view does not exist, belongs to another pipeline, or the user has no access to it ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `categoryId` | Required field `categoryId` is missing | Pass `categoryId`. For a lead and an estimate, pass `0` ||
|| `categoryId` | This request requires data type `int` in field `categoryId` | Pass `categoryId` as a number, not a string ||
|| `categoryId` | Field value must be 0 or greater | Pass `0` or a pipeline ID ||
|| `userId` | Field value must be 1 or greater | Pass the ID of an existing user or do not pass `userId` ||
|| `id` | Required field `id` is missing | Pass the view ID ||
|| `id` | This request requires data type `string` in field `id` | Pass `id` as a string, for example, `"17"` ||
|| `fields` | Required field `fields` is missing | Pass at least one key in `fields`. The error is also returned for an empty `fields` object ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `audience.0.type` | Field value is not one of the possible values: user, department, project, structure-node | Pass one of the member types from the parameter description ||
|| `configuration.0.elements` | Required field `elements` is empty | Pass `name`, `title`, and `elements` in each section ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `LayoutFieldsDto` | Remove the extra key. The request accepts `id`, `categoryId`, `userId`, and `fields` ||
|#

#### Unsupported Pipeline

Error Code: `UNSUPPORTED_CONTEXT`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | This form layout context is not supported | Check `categoryId`: for a lead and an estimate, pass `0`; for other types, pass the ID of an existing pipeline of this object type ||
|#

#### Access Denied

Error Code: `PERMISSION_DENIED`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Insufficient permissions to work with this form layout | The common and additional views are updated by a user with permission to edit the common card view, and another user's personal view by a CRM administrator ||
|#

#### Storage Error

Error Code: `STORAGE_FAILED`, HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Could not complete the operation on the form layout | Retry the request later. If the error persists, contact support ||
|#

#### Changes Partially Applied

Error Code: `CONSISTENCY_FAILURE`, HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Form layout changes were partially applied | Some of the changes were saved. Read the view with the [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) method and repeat the update ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.detailpagelayout.update`. My company has no card view methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-detail-page-layout-add-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-get-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-list-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-delete-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-get-current-rest-v3.md)
