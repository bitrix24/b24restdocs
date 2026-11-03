# Create a CRM Pipeline v 3.0 crm.[type].category.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage pipelines" permission in a CRM access role. For an SPA of a digital workplace — an administrator of this digital workplace or a user with the "Manage pipelines" permission in a digital workplace access role

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.add` method creates a pipeline for a CRM object type and returns its ID. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.add` ||
|| Contact | `crm.contact.category.add` ||
|| Company | `crm.company.category.add` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.add`, for example, `crm.smartprocess1038.category.add` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals, contacts, and companies have pipelines, and so does an SPA if pipelines are enabled in its type settings. Leads, estimates, invoices, and my company have none.

The method name is written in lowercase: `crm.smartprocess1038.category.add`, not `crm.smartProcess1038.category.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new pipeline in the format `{"field": "value"}`. The required field is `name`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

Field names are written in camelCase: `name`, `sort`, `isDefault`.

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Pipeline name, up to 100 characters. The method does not accept an empty string or a string of spaces only ||
|| **sort**
[`integer`](../../../data-types.md) | Pipeline order: the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method returns pipelines in ascending `sort` order. The default value depends on the object type — [object type differences](#entity-differences) ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag. The method accepts the field for a contact, a company, and an SPA — [object type differences](#entity-differences).

Possible values:

- `true` — the new pipeline becomes the default pipeline instead of the previous one
- `false` — the new pipeline is a regular one

Default: `false` ||
|#

The `id` field is read-only, and the method does not accept it. The list of pipeline fields with a flag showing whether a field can be changed is returned by the [crm.&lt;type&gt;.category.field.list](./index.md) method.

### Object Type Differences {#entity-differences}

#|
|| **What differs** | **Deal** | **Contact, company** | **SPA** ||
|| Stages of a new pipeline | Default stages are created | No stages | Default stages are created if stages are enabled in the type settings ||
|| `isDefault` on creation | Read-only: the method returns an error | Can be passed | Can be passed ||
|| Default `sort` | `0` | `500` | `500` ||
|| Negative `sort` | The method stores `0` | The method stores the value as is | The method stores the value as is ||
|| Leading and trailing spaces in `name` | The method trims them | The method retains them | The method retains them ||
|| `isSystem` and `code` pipeline fields | None: the method returns an unknown field error | Present, read-only | Present, read-only ||
|#

The stages that Bitrix24 created in the new pipeline are returned by the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.add`

{% endnote %}

The examples create a pipeline in an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Equipment procurement","sort":600}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Equipment procurement","sort":600},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CategoryAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CategoryAddResult>(
        'crm.smartprocess1038.category.add',
        {
          fields: {
            name: 'Equipment procurement',
            sort: 600,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Category created:', result.id)
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
      async function addCategory() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.add',
            {
              fields: {
                name: 'Equipment procurement',
                sort: 600,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Category created:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCategory)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "name": "Equipment procurement",
                    "sort": 600,
                },
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
                'crm.smartprocess1038.category.add',
                [
                    'fields' => [
                        'name' => 'Equipment procurement',
                        'sort' => 600,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.category.add',
        {
            fields: {
                name: 'Equipment procurement',
                sort: 600,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.category.add',
        [
            'fields' => [
                'name' => 'Equipment procurement',
                'sort' => 600,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.add", b24.Params{
    	"fields": b24.Params{
    		"name": "Equipment procurement",
    		"sort": 600,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.add: %w", err)
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
        "id": 113
    },
    "time": {
        "start": 1793711086,
        "finish": 1793711086.787867,
        "duration": 0.7878670692443848,
        "processing": 0,
        "date_start": "2026-11-03T16:04:46+03:00",
        "date_finish": "2026-11-03T16:04:46+03:00",
        "operating_reset_at": 1793711520,
        "operating": 0.1263599395751953
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
[`integer`](../../../data-types.md) | ID of the created pipeline. It is passed in `categoryId` when creating deal and SPA items and stages. The pipeline fields can be retrieved with the [crm.&lt;type&gt;.category.get](./crm-entity-category-get-rest-v3.md) method ||
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

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `sort` | This request requires data type `int` in field `sort` | Pass a value of the type specified in the field description ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | The name consists of spaces only. Pass a name in `name` that contains more than spaces. The `field` contains the error code, not a field name ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field "name" is empty | Pass `name` in `fields` ||
|| `name` | Field value must not be empty | An empty string. Pass a non-empty string in `name` ||
|| `name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `id`, `isSystem`, `code` | Field "#FIELD#" is read-only | Remove the field from `fields`: it is read-only. For `isSystem` and `code`, this error is returned for a contact, a company, and an SPA ||
|| `isDefault` | Field "isDefault" is read-only | Remove `isDefault` from `fields`. For a deal, the method returns this error because the field is read-only ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the parameter name against the [fields parameter table](#fields). Names are written in camelCase: `name`, not `NAME`. An error with the field name `0` is caused by `fields` passed as a string or a non-empty array instead of an object ||
|| `-` | Unknown field `isSystem` found in entity `CategoryDealDto` | A deal pipeline has no `isSystem` or `code` fields. Remove them from `fields` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator or a user with the "Manage pipelines" permission in an access role ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.add`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-update-rest-v3.md)
- [{#T}](./crm-entity-category-get-rest-v3.md)
- [{#T}](./crm-entity-category-list-rest-v3.md)
- [{#T}](./crm-entity-category-delete-rest-v3.md)
- [{#T}](./crm-entity-category-stage-add-rest-v3.md)
