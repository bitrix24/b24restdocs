# Update a CRM Item v 3.0 crm.[type].update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit items of this CRM object type. For my company, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.update` method updates the fields of an existing CRM item. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.update` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.update` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.update` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.update` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.update` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.update` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.update` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.update`, for example, `crm.smartprocess1038.update` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.update`, not `crm.smartInvoice.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID of the item to update.

The ID is returned by the [crm.&lt;type&gt;.add](./crm-entity-add-rest-v3.md) method when an item is created. IDs of existing items can be retrieved with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with new field values in the format `{"field": "value"}`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

Pass only the fields you need to change. Fields that are not in `fields` retain their current values.

Field names are written in camelCase: `title`, `assignedById`, `stageId`. Each object type has its own set of fields, which are described on these pages:

- [deal fields](./fields-deal.md)
- [lead fields](./fields-lead.md)
- [contact fields](./fields-contact.md)
- [company fields](./fields-company.md)
- [estimate fields](./fields-quote.md)
- [invoice fields](./fields-smart-invoice.md)
- [my company fields](./fields-my-company.md)
- [SPA fields](./fields-smart-process.md)

Not all fields can be edited. The list of fields, with an indication of whether each field can be edited, is returned by the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Custom fields are passed by name, for example, `UF_CRM_1687529546`. Their list is returned by the [crm.&lt;type&gt;.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

The `stageId` value must be a stage of the item's pipeline, and `categoryId` must be an existing pipeline of this object type. Retrieve stages and pipelines with the [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) and [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) methods.

If `fields` is an empty object or all its values match the current ones, the method returns `true` and does not write the item: the modification time `updatedTime` stays the same.

The method does not accept `null` for a string field and returns a data type error. An empty string is handled differently depending on the object type — see [differences between object types](#types).

### Differences Between Object Types {#types}

#|
|| **Object type** | **Empty value in fields** ||
|| Lead, company | An empty `title` is not written: the method returns `true`, and the item name does not change ||
|| Contact | The method does not accept empty `name` and `lastName` at the same time and returns an error. One of the two fields must remain filled in ||
|| SPA | An empty `title` is replaced with the default name: the SPA type name and the item number, for example, `No fields #25` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.update`

{% endnote %}

The examples change the name and amount of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"fields":{"title":"Equipment purchase request","opportunity":180000}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"fields":{"title":"Equipment purchase request","opportunity":180000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ItemUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ItemUpdateResult>(
        'crm.smartprocess1038.update',
        {
          id: 15,
          fields: {
            title: 'Equipment purchase request',
            opportunity: 180000,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Item updated:', result.result)
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
      async function updateItem() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.update',
            {
              id: 15,
              fields: {
                title: 'Equipment purchase request',
                opportunity: 180000,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Item updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateItem)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 15,
                "fields": {
                    "title": "Equipment purchase request",
                    "opportunity": 180000,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.update',
                [
                    'id' => 15,
                    'fields' => [
                        'title' => 'Equipment purchase request',
                        'opportunity' => 180000,
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
        'crm.smartprocess1038.update',
        {
            id: 15,
            fields: {
                title: 'Equipment purchase request',
                opportunity: 180000,
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
        'crm.smartprocess1038.update',
        [
            'id' => 15,
            'fields' => [
                'title' => 'Equipment purchase request',
                'opportunity' => 180000,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.update", b24.Params{
    	"id": 15,
    	"fields": b24.Params{
    		"title":       "Equipment purchase request",
    		"opportunity": 180000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.update: %w", err)
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
        "start": 1793706173,
        "finish": 1793706173.219349,
        "duration": 0.21934890747070312,
        "processing": 0,
        "date_start": "2026-11-03T14:42:53+03:00",
        "date_finish": "2026-11-03T14:42:53+03:00",
        "operating_reset_at": 1793706600,
        "operating": 0.33750319480895996
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
[`boolean`](../../../data-types.md) | `true` if the request was executed. The method also returns `true` when there is nothing to write — see the [fields parameter description](#fields). The new field values can be retrieved with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Required field \"fields\" is missing.",
                "field": "fields"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | Check the item ID. This object type has no item with this `id`, or the item has been deleted ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the item `id` ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `assignedById` | This request requires data type `int` in field `assignedById` | Pass a value of the type specified in the field description. `null` in a string field also causes this error ||
|| `CRM_FIELD_ERROR_REQUIRED` | Fields `#NAME#` and `#LAST_NAME#` cannot be both empty. Please complete at least one of them | Keep `name` or `lastName` filled in. The rule applies to contacts ||
|#

The method also wraps errors that Bitrix24 returned while writing the item into this code. In this case, `field` contains the field name or the write error code, as in the contact example.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Field `id` is read-only | Pass the ID in the `id` parameter, not in `fields` ||
|| `createdTime` | Field `createdTime` is read-only | Remove fields that cannot be edited from `fields`. Whether a field can be edited is returned by [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) ||
|| `stageId` | Field value is not one of the possible values: `#STAGES#` | Pass a stage from the item's pipeline. The error message lists the allowed stages ||
|| `categoryId` | Field value is not one of the possible values | Pass the ID of an existing pipeline of this object type ||
|| `assignedById` | Field value is not one of the possible values | Pass the ID of an existing user ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name on the fields page of the object type. An error with the field name `0` occurs when `fields` is passed as a string or an array instead of an object ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.update`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-add-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](./crm-entity-delete-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
