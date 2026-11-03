# Remove a Field Requirement v 3.0 crm.[type].field.required.clear

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.field.required.clear` method removes the field requirement in a pipeline: after the call, the requirement mode is `none`. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.required.clear` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.required.clear` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.required.clear` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.required.clear` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.required.clear` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.required.clear` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.required.clear`, for example, `crm.smartprocess1038.field.required.clear` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have field requirement methods regardless of the type settings. My company does not have these methods: for `crm.mycompany.field.required.clear`, Bitrix24 returns the ["Method Not Found"](#method-not-found) error.

The method name is written in lowercase: `crm.smartinvoice.field.required.clear`, not `crm.smartInvoice.field.required.clear`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fieldId***
[`string`](../../../data-types.md) | Field name: a standard field in camelCase, for example, `title` or `opportunity`, or a custom field, for example, `UF_CRM_CONTRACT_NUMBER`. Field names are returned by the [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) method ||
|| **categoryId**
[`integer`](../../../data-types.md) | Pipeline ID. [When to pass it](#category-id) ||
|#

### The categoryId Parameter {#category-id}

Bitrix24 stores the field requirement separately for each pipeline. Whether to pass `categoryId` depends on the object type:

#|
|| **Object type** | **categoryId** ||
|| Deal, contact, company | Required. The default pipeline is set by `0`, other pipelines by their IDs from the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method ||
|| SPA | Required. The pipeline ID from the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method. If pipelines are disabled in the type, the SPA has no pipeline methods: the ID of its only pipeline is returned by the [crm.category.list](../../universal/category/crm-category-list.md) method with the `entityTypeId` of the SPA ||
|| Invoice | Required. The invoice pipeline ID is returned by the [crm.category.list](../../universal/category/crm-category-list.md) method with `entityTypeId` = `31`: REST 3.0 has no pipeline method for invoices ||
|| Lead, estimate | Not passed ||
|#

If `categoryId` does not follow the rules in the table, the method returns an error with the text `Invalid category ID.`

The method does not change the `isMandatory` attribute of a custom field: you can clear it with the [crm.&lt;type&gt;.field.custom.update](./crm-entity-field-custom-update-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.required.clear`

{% endnote %}

The examples remove the requirement of the "Contract number" field in pipeline `69` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fieldId":"UF_CRM_CONTRACT_NUMBER","categoryId":69}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.required.clear
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fieldId":"UF_CRM_CONTRACT_NUMBER","categoryId":69,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.required.clear
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RequirementClearResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RequirementClearResult>(
        'crm.smartprocess1038.field.required.clear',
        {
          fieldId: 'UF_CRM_CONTRACT_NUMBER',
          categoryId: 69,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Requirement cleared:', result.result)
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
      async function clearRequirement() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.required.clear',
            {
              fieldId: 'UF_CRM_CONTRACT_NUMBER',
              categoryId: 69,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Requirement cleared:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', clearRequirement)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.required.clear"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fieldId": "UF_CRM_CONTRACT_NUMBER",
                "categoryId": 69,
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.field.required.clear',
                [
                    'fieldId' => 'UF_CRM_CONTRACT_NUMBER',
                    'categoryId' => 69,
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
        'crm.smartprocess1038.field.required.clear',
        {
            fieldId: 'UF_CRM_CONTRACT_NUMBER',
            categoryId: 69,
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
        'crm.smartprocess1038.field.required.clear',
        [
            'fieldId' => 'UF_CRM_CONTRACT_NUMBER',
            'categoryId' => 69,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.required.clear", b24.Params{
    	"fieldId":    "UF_CRM_CONTRACT_NUMBER",
    	"categoryId": 69,
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.required.clear: %w", err)
    }

    var out struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Result)
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
        "start": 1793716022,
        "finish": 1793716022.250977,
        "duration": 0.2509770393371582,
        "processing": 0,
        "date_start": "2026-11-03T17:27:02+03:00",
        "date_finish": "2026-11-03T17:27:02+03:00",
        "operating_reset_at": 1793716620,
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
[`boolean`](../../../data-types.md) | `true` if the requirement is removed or was not set ||
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
                "message": "Invalid category ID.",
                "field": "categoryId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

The "Pipeline Not Found" error is returned with the short code `CONTEXT_NOT_FOUND` without the `BITRIX_REST_V3_` prefix. Its text and the texts of validation errors are in English.

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fieldId` | fieldId must be a non-empty string. | Pass the field name as a string, for example, `"UF_CRM_CONTRACT_NUMBER"` ||
|| `categoryId` | Invalid category ID. | Pass `categoryId` according to the [parameter table](#category-id) ||
|| `categoryId` | categoryId must be a non-negative integer. | Pass the pipeline ID as an integer of `0` or greater ||
|#

#### Field Not Found

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#FIELD#` was not found | Check the field name with the [crm.&lt;type&gt;.field.list](./crm-entity-field-list-rest-v3.md) method. Standard field names are written in camelCase: `title`, not `TITLE` ||
|#

#### Pipeline Not Found

Error Code: `CONTEXT_NOT_FOUND`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The field requirement context was not found. | Check `categoryId` against the [parameter table](#category-id) ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a user with the "Edit CRM preferences" permission in CRM ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.required.clear`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company does not have these methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-required-get-rest-v3.md)
- [{#T}](./crm-entity-field-required-set-rest-v3.md)
