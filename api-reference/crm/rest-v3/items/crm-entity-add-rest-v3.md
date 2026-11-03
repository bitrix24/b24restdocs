# Create a CRM Item v 3.0 crm.[type].add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to add items of this CRM object type. For my company, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.add` method creates a CRM item and returns its ID. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.add` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.add` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.add` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.add` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.add` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.add` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.add` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.add`, for example, `crm.smartprocess1038.add` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.add`, not `crm.smartInvoice.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new item in the format `{"field": "value"}`. [Parameter description](#fields).

Leads, contacts, companies, and my companies have [required fields](#required-fields) ||
|#

### The fields Parameter {#fields}

Field names are written in camelCase: `title`, `assignedById`, `stageId`. The method does not accept uppercase names such as `TITLE` and returns an unknown field error. Pass only fields that can be edited. Each object type has its own set of fields, which are described on these pages:

- [deal fields](./fields-deal.md)
- [lead fields](./fields-lead.md)
- [contact fields](./fields-contact.md)
- [company fields](./fields-company.md)
- [estimate fields](./fields-quote.md)
- [invoice fields](./fields-smart-invoice.md)
- [my company fields](./fields-my-company.md)
- [SPA fields](./fields-smart-process.md)

The list of fields, with an indication of whether each field can be edited, is returned by the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

Custom fields are passed by name, for example, `UF_CRM_1687529546`. Their list is returned by the [crm.&lt;type&gt;.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

The `stageId` value must be a stage of the item's pipeline, and `categoryId` must be an existing pipeline of this object type. Retrieve stages and pipelines with the [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) and [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) methods.

What the method sets if a field is not passed:

- without `categoryId`, the item is created in the default pipeline
- without `stageId`, the item is placed at the first stage of the pipeline. If only `stageId` is passed, it is validated against the stages of the default pipeline
- without `title`, the name of an SPA item, estimate, or invoice is generated from the type name and the ID, for example, "Purchases #35" or "Estimate #147"

Product rows are not passed in `fields`: add them with the [crm.&lt;type&gt;.productrow.*](../product-rows/crm-entity-product-row-list-rest-v3.md) methods after the item is created.

### Required Fields {#required-fields}

Without these fields, the method returns an error and does not create the item:

#|
|| **Object type** | **Required** ||
|| Lead, company, my company | `title` ||
|| Contact | `name` or `lastName` — at least one of the two ||
|| Deal, estimate, invoice, SPA | No required fields. The item is created even with an empty `fields` object ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.add`

{% endnote %}

The examples create an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Purchase request","assignedById":1,"opportunity":150000,"currencyId":"EUR"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Purchase request","assignedById":1,"opportunity":150000,"currencyId":"EUR"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ItemAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ItemAddResult>(
        'crm.smartprocess1038.add',
        {
          fields: {
            title: 'Purchase request',
            assignedById: 1,
            opportunity: 150000,
            currencyId: 'EUR',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Item created:', result.id)
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
      async function addItem() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.add',
            {
              fields: {
                title: 'Purchase request',
                assignedById: 1,
                opportunity: 150000,
                currencyId: 'EUR',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Item created:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addItem)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "title": "Purchase request",
                    "assignedById": 1,
                    "opportunity": 150000,
                    "currencyId": "EUR",
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
                'crm.smartprocess1038.add',
                [
                    'fields' => [
                        'title' => 'Purchase request',
                        'assignedById' => 1,
                        'opportunity' => 150000,
                        'currencyId' => 'EUR',
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
        'crm.smartprocess1038.add',
        {
            fields: {
                title: 'Purchase request',
                assignedById: 1,
                opportunity: 150000,
                currencyId: 'EUR',
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
        'crm.smartprocess1038.add',
        [
            'fields' => [
                'title' => 'Purchase request',
                'assignedById' => 1,
                'opportunity' => 150000,
                'currencyId' => 'EUR',
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.add", b24.Params{
    	"fields": b24.Params{
    		"title":        "Purchase request",
    		"assignedById": 1,
    		"opportunity":  150000,
    		"currencyId":   "EUR",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.add: %w", err)
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
        "id": 7
    },
    "time": {
        "start": 1793705270,
        "finish": 1793705272.058552,
        "duration": 2.0585520267486572,
        "processing": 1,
        "date_start": "2026-11-03T14:27:50+03:00",
        "date_finish": "2026-11-03T14:27:52+03:00",
        "operating_reset_at": 1793705820,
        "operating": 0.8111288547515869
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
[`integer`](../../../data-types.md) | ID of the created item. The item fields can be retrieved with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method ||
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
                "message": "Field value is not one of the possible values: DT1038_69:NEW, DT1038_69:PREPARATION, DT1038_69:CLIENT, DT1038_69:SUCCESS, DT1038_69:FAIL",
                "field": "stageId"
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
|| `assignedById` | This request requires data type `int` in field `assignedById` | Pass a value of the type specified in the field description ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `title` | Required field `title` is empty | Pass `title` in `fields`. The field is required for leads, companies, and my companies ||
|| `-` | At least one of name or lastName is required | Pass `name` or `lastName` in `fields`. The rule applies to contacts ||
|| `id` | Field `id` is read-only | Remove `id` from `fields`: the ID is assigned automatically ||
|| `stageId` | Field value is not one of the possible values: `#STAGES#` | Pass a stage from the item's pipeline. The error message lists the allowed stages ||
|| `categoryId` | Field value is not one of the possible values | Pass the ID of an existing pipeline of this object type ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name on the fields page of the object type. Field names are written in camelCase: `title`, not `TITLE`. An error with the field name `0` occurs when `fields` is passed as a string or an array instead of an object ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.add`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](./crm-entity-delete-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
