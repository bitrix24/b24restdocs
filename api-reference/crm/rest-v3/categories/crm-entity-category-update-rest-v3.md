# Update a CRM Pipeline v 3.0 crm.[type].category.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage pipelines" permission in a CRM access role. For an SPA of a digital workplace — an administrator of this digital workplace or a user with the "Manage pipelines" permission in a digital workplace access role. The user must also have permission to read the items of this pipeline

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.update` method updates the name, order, and default flag of an existing pipeline. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.update` ||
|| Contact | `crm.contact.category.update` ||
|| Company | `crm.company.category.update` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.update`, for example, `crm.smartprocess1038.category.update` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals, contacts, and companies have pipelines, and so does an SPA if pipelines are enabled in its type settings. Leads, estimates, invoices, and my company have none.

The method name is written in lowercase: `crm.smartprocess1038.category.update`, not `crm.smartProcess1038.category.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID of the pipeline to update. For the default pipeline of a deal, a contact, and a company, it is `0`.

It is returned by the [crm.&lt;type&gt;.category.add](./crm-entity-category-add-rest-v3.md) method when a pipeline is created. IDs of existing pipelines can be retrieved with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with the new field values in the format `{"field": "value"}`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

Pass only the fields you want to change. Fields that are not in `fields` retain their current values. The method accepts an empty `fields` object and changes nothing.

Field names are written in camelCase: `name`, `sort`, `isDefault`.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Pipeline name, up to 100 characters. The method does not accept an empty string or a string of spaces only ||
|| **sort**
[`integer`](../../../data-types.md) | Pipeline order: the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method returns pipelines in ascending `sort` order ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag. The method accepts the field for a contact, a company, and an SPA — [object type differences](#entity-differences).

Possible values:

- `true` — the pipeline becomes the default pipeline, and the flag is removed from the previous one
- `false` — the pipeline becomes a regular one ||
|#

The method does not accept the `id` field in `fields`: the pipeline is specified by the `id` parameter. The list of pipeline fields with a flag showing whether a field can be changed is returned by the [crm.&lt;type&gt;.category.field.list](./index.md) method.

### Object Type Differences {#entity-differences}

#|
|| **What differs** | **Deal** | **Contact, company** | **SPA** ||
|| `isDefault` | Read-only: the method returns an error | Can be passed | Can be passed ||
|| Negative `sort` | The method stores `0` | The method stores the value as is | The method stores the value as is ||
|| Leading and trailing spaces in `name` | The method trims them | The method retains them | The method retains them ||
|| `isSystem` and `code` fields | None: the method returns an unknown field error | Present, read-only | Present, read-only ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.update`

{% endnote %}

The examples change the name and order of pipeline `105` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":105,"fields":{"name":"Equipment and furniture procurement","sort":650}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":105,"fields":{"name":"Equipment and furniture procurement","sort":650},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CategoryUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CategoryUpdateResult>(
        'crm.smartprocess1038.category.update',
        {
          id: 105,
          fields: {
            name: 'Equipment and furniture procurement',
            sort: 650,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Category updated:', result.result)
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
      async function updateCategory() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.update',
            {
              id: 105,
              fields: {
                name: 'Equipment and furniture procurement',
                sort: 650,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Category updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCategory)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 105,
                "fields": {
                    "name": "Equipment and furniture procurement",
                    "sort": 650,
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
                'crm.smartprocess1038.category.update',
                [
                    'id' => 105,
                    'fields' => [
                        'name' => 'Equipment and furniture procurement',
                        'sort' => 650,
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
        'crm.smartprocess1038.category.update',
        {
            id: 105,
            fields: {
                name: 'Equipment and furniture procurement',
                sort: 650,
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
        'crm.smartprocess1038.category.update',
        [
            'id' => 105,
            'fields' => [
                'name' => 'Equipment and furniture procurement',
                'sort' => 650,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.update", b24.Params{
    	"id":     105,
    	"fields": b24.Params{
    		"name": "Equipment and furniture procurement",
    		"sort": 650,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.update: %w", err)
    }

    var updated struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &updated); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(updated.Result)
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
        "start": 1793714729,
        "finish": 1793714729.963392,
        "duration": 0.9633920192718506,
        "processing": 0,
        "date_start": "2026-11-03T17:05:29+03:00",
        "date_finish": "2026-11-03T17:05:29+03:00",
        "operating_reset_at": 1793715300,
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
[`boolean`](../../../data-types.md) | `true` if the request was completed. The method also returns `true` for an empty `fields` — see the [fields parameter description](#fields). The new values can be retrieved with the [crm.&lt;type&gt;.category.get](./crm-entity-category-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pipeline Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check the pipeline ID with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method. This object type has no pipeline with this `id`, or the user has no permission to read its items ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the pipeline `id` ||
|| `id` | Field value must be 0 or greater | Pass a non-negative pipeline ID ||
|| `id` | This request requires data type `int` in field `id` | Pass `id` as a number ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | The name consists of spaces only. Pass a name in `name` that contains more than spaces. The `field` contains the error code, not a field name ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field value must not be empty | An empty string. Pass a non-empty string in `name` ||
|| `name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `id`, `isSystem`, `code` | Field "#FIELD#" is read-only | Pass the ID in the `id` parameter, not in `fields`. The `isSystem` and `code` fields cannot be changed ||
|| `isDefault` | Field "isDefault" is read-only | Remove `isDefault` from `fields`. For a deal, the method returns this error because the field is read-only ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the parameter name against the [fields parameter table](#fields). Names are written in camelCase: `name`, not `NAME` ||
|| `-` | Unknown field `code` found in entity `CategoryDealDto` | A deal pipeline has no `isSystem` or `code` fields. Remove them from `fields` ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.update`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-add-rest-v3.md)
- [{#T}](./crm-entity-category-get-rest-v3.md)
- [{#T}](./crm-entity-category-list-rest-v3.md)
- [{#T}](./crm-entity-category-delete-rest-v3.md)
