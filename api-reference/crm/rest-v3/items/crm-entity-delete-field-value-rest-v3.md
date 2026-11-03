# Remove a Value from a Multiple Field of a CRM Item v 3.0 crm.[type].deletefieldvalue

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

The `crm.<type>.deletefieldvalue` method removes a single value from a multiple field of a CRM item. The other values of the field are kept. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.deletefieldvalue` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.deletefieldvalue` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.deletefieldvalue` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.deletefieldvalue` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.deletefieldvalue` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.deletefieldvalue` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.deletefieldvalue` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.deletefieldvalue`, for example, `crm.smartprocess1038.deletefieldvalue` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.deletefieldvalue`, not `crm.smartInvoice.deletefieldvalue`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Supported Fields {#supported-fields}

The method works with the same fields as [crm.&lt;type&gt;.addfieldvalue](./crm-entity-add-field-value-rest-v3.md). Other fields, for example, `title` or `companyId`, cause the `CRM_FIELD_NOT_SUPPORTED` error.

#|
|| **Field** | **What it removes** | **Object types** ||
|| `phone`, `email`, `web`, `im` | A phone number, email, website, or messenger | Lead, contact, company, my company ||
|| `contactsId` | A contact link | Deal, lead, company, estimate, invoice, my company, SPA ||
|| `companiesId` | A company link | Contact ||
|| `observersId` | An observer | Deal, lead, contact, company, invoice, my company, SPA ||
|| Multiple custom field `UF_CRM_…` | A custom field value | All types from the table above ||
|#

An SPA has the `contactsId` field if client linking is enabled for the type, and `observersId` if observers are enabled. For details, see the [SPA fields](./fields-smart-process.md) page.

A single-value custom field causes the `CRM_FIELD_NOT_MULTIPLE` error. The method does not support a custom field of the "Yes/No" type, even if it is multiple. The `multiple` attribute of a field is returned by the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Item ID. Item IDs can be retrieved with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method ||
|| **fieldName***
[`string`](../../../data-types.md) | Name of the field to remove the value from. Supported fields are listed in the [Supported Fields](#supported-fields) section ||
|| **value***
[`any`](../../../data-types.md) | Value to remove. The format depends on the field. [Parameter description](#value) ||
|#

### The value Parameter {#value}

The value is passed in the same format as in [crm.&lt;type&gt;.addfieldvalue](./crm-entity-add-field-value-rest-v3.md#value):

#|
|| **Field** | **Value format** ||
|| `phone`, `email`, `web`, `im` | Object `{"valueTypeId": "WORK", "value": "+49300000000"}`. The value whose `valueTypeId` and `value` both match is removed ||
|| `contactsId` | ID of a linked contact, a number ||
|| `companiesId` | ID of a linked company, a number ||
|| `observersId` | ID of an observer, a number ||
|| Custom field | A single value in the format of the field type, for example, a string for a string field or an option ID for a list ||
|#

The current field values are returned by the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method. If the value is not in the field, the method returns the `CRM_FIELD_VALUE_NOT_FOUND` error and does not change the item.

If you remove the primary contact, the next linked contact becomes the primary one: its ID is returned in the `contactId` field. You can choose the primary contact with the [crm.&lt;type&gt;.setprimarycontact](./crm-entity-set-primary-contact-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.deletefieldvalue`

{% endnote %}

The examples remove a contact link from an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"fieldName":"contactsId","value":3215}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.deletefieldvalue
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"fieldName":"contactsId","value":3215,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.deletefieldvalue
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DeleteFieldValueResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<DeleteFieldValueResult>(
        'crm.smartprocess1038.deletefieldvalue',
        {
          id: 27,
          fieldName: 'contactsId',
          value: 3215,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Value deleted:', result.result)
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
      async function deleteFieldValue() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.deletefieldvalue',
            {
              id: 27,
              fieldName: 'contactsId',
              value: 3215,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Value deleted:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteFieldValue)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.deletefieldvalue"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 27,
                "fieldName": "contactsId",
                "value": 3215,
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
                'crm.smartprocess1038.deletefieldvalue',
                [
                    'id' => 27,
                    'fieldName' => 'contactsId',
                    'value' => 3215,
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
        'crm.smartprocess1038.deletefieldvalue',
        {
            id: 27,
            fieldName: 'contactsId',
            value: 3215,
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
        'crm.smartprocess1038.deletefieldvalue',
        [
            'id' => 27,
            'fieldName' => 'contactsId',
            'value' => 3215,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.deletefieldvalue", b24.Params{
    	"id":        27,
    	"fieldName": "contactsId",
    	"value":     3215,
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.deletefieldvalue: %w", err)
    }

    var deleted struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &deleted); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(deleted.Result)
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
        "start": 1793706766,
        "finish": 1793706767.007953,
        "duration": 1.0079529285430908,
        "processing": 1,
        "date_start": "2026-11-03T14:52:46+03:00",
        "date_finish": "2026-11-03T14:52:47+03:00",
        "operating_reset_at": 1793707320,
        "operating": 0.12220501899719238
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
[`boolean`](../../../data-types.md) | `true` if the value is removed. The new field contents can be retrieved with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method ||
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
                "message": "The field value was not found.",
                "field": "CRM_FIELD_VALUE_NOT_FOUND"
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
|| `-` | Entity with ID #ID# was not found | Check the item ID. An item with this `id` does not exist for this object type or has been deleted ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

Request parameter errors:

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the item `id` ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `fieldName` | Required field `fieldName` is missing | Pass the field name ||
|| `fieldName` | Field value must not be empty | Pass a non-empty field name ||
|| `value` | Required field `value` is missing | Pass the value. `null` also causes this error ||
|| `value` | Field value must not be empty | Pass a non-empty value ||
|#

Value errors. In these errors, the `field` field contains an error code instead of a field name:

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `CRM_FIELD_NOT_SUPPORTED` | The field is not supported | The method does not work with this field for this object type. Check the field against the [Supported Fields](#supported-fields) section ||
|| `CRM_FIELD_NOT_MULTIPLE` | Only multiple fields are supported | The custom field is not multiple. Clear it with the [crm.&lt;type&gt;.update](./crm-entity-update-rest-v3.md) method ||
|| `CRM_FIELD_VALUE_NOT_FOUND` | The field value was not found | The field does not contain this value. Check the current values with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method ||
|| `CRM_FIELD_VALUE_INVALID` | The relation value does not exist | For `companiesId`: a company with this ID is not linked to the contact ||
|| `CRM_FIELD_VALUE_LOCKED` | The field is being changed by another operation | Another request is changing this item field at the moment. Retry the call later ||
|| `-` | The field value is invalid | The value has the wrong format: a string or `0` instead of an ID, a phone object without `valueTypeId` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.deletefieldvalue`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-add-field-value-rest-v3.md)
- [{#T}](./crm-entity-set-primary-contact-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
