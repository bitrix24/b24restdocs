# Create an Additional Card View v 3.0 crm.[type].detailpagelayout.add

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

The `crm.<type>.detailpagelayout.add` method creates an additional card view for CRM items and returns its ID. The object type is set by the `<type>` segment of the method name.

An additional view is a set of card sections and fields with a name and a list of members who can access it. A member can select the view in the card, or the view can be assigned with the [crm.&lt;type&gt;.detailpagelayout.setcurrent](./crm-entity-detail-page-layout-set-current-rest-v3.md) method. After creation, the view does not become current for anyone.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.detailpagelayout.add` ||
|| Lead | `crm.lead.detailpagelayout.add` ||
|| Contact | `crm.contact.detailpagelayout.add` ||
|| Company | `crm.company.detailpagelayout.add` ||
|| Estimate | `crm.quote.detailpagelayout.add` ||
|| Invoice | `crm.smartinvoice.detailpagelayout.add` ||
|| CRM document | `crm.smartdocument.detailpagelayout.add` ||
|| B2E document | `crm.smartb2edocument.detailpagelayout.add` ||
|| SPA | `crm.smartprocess<entityTypeId>.detailpagelayout.add`, for example, `crm.smartprocess1038.detailpagelayout.add` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. Type IDs can be retrieved with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. Card view methods are available for an SPA regardless of the type settings. My company has no such methods.

The method name is written in lowercase: `crm.deal.detailpagelayout.add`, not `crm.deal.detailPageLayout.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **categoryId***
[`integer`](../../../data-types.md) | Pipeline ID. Card views are stored separately for each pipeline.

The value depends on the object type:

- lead and estimate — only `0`, as they have no pipelines
- deal, contact, and company — the pipeline ID; `0` is the default pipeline
- invoice, documents, and SPA — the pipeline ID. The method replaces the value `0` with the ID of the default pipeline

Pipeline IDs of deals, contacts, companies, and SPAs can be retrieved with the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Fields of the new view [(detailed description)](#fields) ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | View name, up to 100 characters. The method trims leading and trailing spaces ||
|| **configuration***
[`object[]`](../../../data-types.md) | Card sections in display order, at least one section [(detailed description)](#configuration) ||
|| **audience***
[`object[]`](../../../data-types.md) | Members who can access the view, at least one [(detailed description)](#audience). The method adds the user who creates the view to the audience automatically ||
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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.detailpagelayout.add`

{% endnote %}

The examples create an additional card view for an SPA with the type ID `1038` in the default pipeline. The view with a single section is available to the department with ID `47`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"fields":{"name":"Procurement team card","configuration":[{"name":"main","title":"About the request","elements":[{"name":"TITLE"},{"name":"STAGE_ID"},{"name":"OPPORTUNITY_WITH_CURRENCY"},{"name":"ASSIGNED_BY_ID"}]}],"audience":[{"type":"department","id":47}]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.detailpagelayout.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":0,"fields":{"name":"Procurement team card","configuration":[{"name":"main","title":"About the request","elements":[{"name":"TITLE"},{"name":"STAGE_ID"},{"name":"OPPORTUNITY_WITH_CURRENCY"},{"name":"ASSIGNED_BY_ID"}]}],"audience":[{"type":"department","id":47}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.detailpagelayout.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type LayoutAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<LayoutAddResult>(
        'crm.smartprocess1038.detailpagelayout.add',
        {
          categoryId: 0,
          fields: {
            name: 'Procurement team card',
            configuration: [
              {
                name: 'main',
                title: 'About the request',
                elements: [
                  { name: 'TITLE' },
                  { name: 'STAGE_ID' },
                  { name: 'OPPORTUNITY_WITH_CURRENCY' },
                  { name: 'ASSIGNED_BY_ID' },
                ],
              },
            ],
            audience: [
              { type: 'department', id: 47 },
            ],
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Layout created:', result.id)
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
      async function addLayout() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.detailpagelayout.add',
            {
              categoryId: 0,
              fields: {
                name: 'Procurement team card',
                configuration: [
                  {
                    name: 'main',
                    title: 'About the request',
                    elements: [
                      { name: 'TITLE' },
                      { name: 'STAGE_ID' },
                      { name: 'OPPORTUNITY_WITH_CURRENCY' },
                      { name: 'ASSIGNED_BY_ID' },
                    ],
                  },
                ],
                audience: [
                  { type: 'department', id: 47 },
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
          console.info('Layout created:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addLayout)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.detailpagelayout.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "categoryId": 0,
                "fields": {
                    "name": "Procurement team card",
                    "configuration": [
                        {
                            "name": "main",
                            "title": "About the request",
                            "elements": [
                                {"name": "TITLE"},
                                {"name": "STAGE_ID"},
                                {"name": "OPPORTUNITY_WITH_CURRENCY"},
                                {"name": "ASSIGNED_BY_ID"},
                            ],
                        },
                    ],
                    "audience": [
                        {"type": "department", "id": 47},
                    ],
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["id"])
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
                'crm.smartprocess1038.detailpagelayout.add',
                [
                    'categoryId' => 0,
                    'fields' => [
                        'name' => 'Procurement team card',
                        'configuration' => [
                            [
                                'name' => 'main',
                                'title' => 'About the request',
                                'elements' => [
                                    ['name' => 'TITLE'],
                                    ['name' => 'STAGE_ID'],
                                    ['name' => 'OPPORTUNITY_WITH_CURRENCY'],
                                    ['name' => 'ASSIGNED_BY_ID'],
                                ],
                            ],
                        ],
                        'audience' => [
                            ['type' => 'department', 'id' => 47],
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
        'crm.smartprocess1038.detailpagelayout.add',
        {
            categoryId: 0,
            fields: {
                name: 'Procurement team card',
                configuration: [
                    {
                        name: 'main',
                        title: 'About the request',
                        elements: [
                            { name: 'TITLE' },
                            { name: 'STAGE_ID' },
                            { name: 'OPPORTUNITY_WITH_CURRENCY' },
                            { name: 'ASSIGNED_BY_ID' },
                        ],
                    },
                ],
                audience: [
                    { type: 'department', id: 47 },
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
        'crm.smartprocess1038.detailpagelayout.add',
        [
            'categoryId' => 0,
            'fields' => [
                'name' => 'Procurement team card',
                'configuration' => [
                    [
                        'name' => 'main',
                        'title' => 'About the request',
                        'elements' => [
                            ['name' => 'TITLE'],
                            ['name' => 'STAGE_ID'],
                            ['name' => 'OPPORTUNITY_WITH_CURRENCY'],
                            ['name' => 'ASSIGNED_BY_ID'],
                        ],
                    ],
                ],
                'audience' => [
                    ['type' => 'department', 'id' => 47],
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.detailpagelayout.add", b24.Params{
    	"categoryId": 0,
    	"fields": b24.Params{
    		"name": "Procurement team card",
    		"configuration": []b24.Params{
    			{
    				"name":  "main",
    				"title": "About the request",
    				"elements": []b24.Params{
    					{"name": "TITLE"},
    					{"name": "STAGE_ID"},
    					{"name": "OPPORTUNITY_WITH_CURRENCY"},
    					{"name": "ASSIGNED_BY_ID"},
    				},
    			},
    		},
    		"audience": []b24.Params{
    			{"type": "department", "id": 47},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.detailpagelayout.add: %w", err)
    }

    var created struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &created); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(created.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 17
    },
    "time": {
        "start": 1793706619,
        "finish": 1793706619.389998,
        "duration": 0.3899979591369629,
        "processing": 0,
        "date_start": "2026-11-03T14:50:19+03:00",
        "date_finish": "2026-11-03T14:50:19+03:00",
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
|| **result.id**
[`integer`](../../../data-types.md) | ID of the created view. In other methods, it is passed as a string, for example, `"17"`. The view can be retrieved with the [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"name\" is empty.",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

Card view errors are returned with short codes without the `BITRIX_REST_V3_` prefix, for example, `LAYOUT_NOT_FOUND`, and with English messages. The HTTP status of these errors varies and is given next to the code. Errors with `BITRIX_REST_V3_*` codes are returned with status 400.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is empty | Pass `name`, `configuration`, and `audience` in `fields` ||
|| `configuration.0.elements` | Required field `elements` is empty | Pass `name`, `title`, and `elements` in each section. For a section without fields, pass an empty `elements` array ||
|| `audience.0.type` | Field value is not one of the possible values: user, department, project, structure-node | Pass one of the member types from the parameter description ||
|| `audience.0.id` | Field value must be 1 or greater | Pass a positive member ID ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `categoryId` | Required field `categoryId` is missing | Pass `categoryId`. For a lead and an estimate, pass `0` ||
|| `categoryId` | This request requires data type `int` in field `categoryId` | Pass `categoryId` as a number, not a string ||
|| `categoryId` | Field value must be 0 or greater | Pass `0` or a pipeline ID ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object with the view fields ||
|| `fields.audience` | Required field `fields.audience` is missing | Pass at least one member in `audience` ||
|| `configuration` | This request requires data type `array` in field `configuration` | Pass `configuration` as an array of sections, not an object ||
|| `audience.0.id` | This request requires data type `int` in field `audience.0.id` | Pass the member ID as a number ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `LayoutFieldsDto` | Remove the extra key. The request accepts `categoryId` and `fields`, and `fields` accepts `name`, `configuration`, and `audience` ||
|| `-` | Unknown field `configuration.0.elements.0.#FIELD#` found in entity `ConfigurationElementDto` | Keep only `name`, `optionFlags`, and `options` in a section field ||
|#

#### Invalid View Configuration

Error Code: `INVALID_LAYOUT_CONFIGURATION`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout configuration | Check `name` — from 1 to 100 characters, and `configuration` — at least one section ||
|#

#### Invalid View Audience

Error Code: `INVALID_LAYOUT_AUDIENCE`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid form layout audience | Check the members: no more than 1000, each with a type from the list and a positive ID ||
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
|| `-` | Insufficient permissions to work with this form layout | Views can be created by a user with permission to edit the common card view of this object type ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.detailpagelayout.add`. My company has no card view methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-detail-page-layout-update-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-get-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-list-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-delete-rest-v3.md)
- [{#T}](./crm-entity-detail-page-layout-set-current-rest-v3.md)
