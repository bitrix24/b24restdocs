# Update a List Field Option v 3.0 crm.[type].field.custom.enum.update

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

The `crm.<type>.field.custom.enum.update` method updates an option of a custom list field and returns the option description. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.custom.enum.update` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.custom.enum.update` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.custom.enum.update` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.custom.enum.update` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.custom.enum.update` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.custom.enum.update` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.custom.enum.update`, for example, `crm.smartprocess1038.field.custom.enum.update` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have list field option methods regardless of the type settings. My company does not have these methods: for `crm.mycompany.field.custom.enum.update`, Bitrix24 returns the ["Method Not Found"](#method-not-found) error.

The method name is written in lowercase: `crm.smartinvoice.field.custom.enum.update`, not `crm.smartInvoice.field.custom.enum.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Option ID. Option IDs are returned by the [crm.&lt;type&gt;.field.custom.enum.list](./crm-entity-field-custom-enum-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with the new option data in the format `{"parameter": "value"}`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

Pass only the parameters you want to change.

#|
|| **Name**
`type` | **Description** ||
|| **value**
[`string`](../../../data-types.md) | Option text that the user sees, for example, `Urgent`. The method does not accept an empty string ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Whether the option is selected by default for new items.

Possible values:

- `true` — the option is selected by default
- `false` — not selected ||
|| **sort**
[`integer`](../../../data-types.md) | Option position in the list ||
|| **xmlId**
[`string`](../../../data-types.md) | Option external code, unique within the field ||
|#

An option cannot be moved to another field: the method skips `fieldId` without an error. The method also accepts an empty `fields` object and returns the option unchanged.

{% note warning "" %}

The method does not clear the `isDefault` flag on other options. If the field already has a default option and you pass `"isDefault": true` for another one, both options are marked as default. To keep only one, pass `"isDefault": false` for the previous option.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.custom.enum.update`

{% endnote %}

The examples change the text and position of an option in the "Priority" field of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4135,"fields":{"value":"Very urgent","sort":1}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.custom.enum.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4135,"fields":{"value":"Very urgent","sort":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.custom.enum.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumUpdateResult = {
      item: {
        id: number
        fieldId: number
        value: string
        isDefault: boolean
        sort: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<EnumUpdateResult>(
        'crm.smartprocess1038.field.custom.enum.update',
        {
          id: 4135,
          fields: {
            value: 'Very urgent',
            sort: 1,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Value updated:', result.item.id, result.item.value)
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
      async function updateEnumValue() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.custom.enum.update',
            {
              id: 4135,
              fields: {
                value: 'Very urgent',
                sort: 1,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Value updated:', result.item.id, result.item.value)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateEnumValue)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.custom.enum.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 4135,
                "fields": {
                    "value": "Very urgent",
                    "sort": 1,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"]["id"], result["item"]["value"])
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
                'crm.smartprocess1038.field.custom.enum.update',
                [
                    'id' => 4135,
                    'fields' => [
                        'value' => 'Very urgent',
                        'sort' => 1,
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.field.custom.enum.update',
        {
            id: 4135,
            fields: {
                value: 'Very urgent',
                sort: 1,
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.field.custom.enum.update',
        [
            'id' => 4135,
            'fields' => [
                'value' => 'Very urgent',
                'sort' => 1,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.custom.enum.update", b24.Params{
    	"id": 4135,
    	"fields": b24.Params{
    		"value": "Very urgent",
    		"sort":  1,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.custom.enum.update: %w", err)
    }

    var out struct {
    	Item struct {
    		ID    int    `json:"id"`
    		Value string `json:"value"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Item.ID, out.Item.Value)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 4135,
            "fieldId": 6008205,
            "value": "Very urgent",
            "isDefault": false,
            "sort": 1,
            "xmlId": "URGENT"
        }
    },
    "time": {
        "start": 1793714989,
        "finish": 1793714989.621933,
        "duration": 0.6219329833984375,
        "processing": 0,
        "date_start": "2026-11-03T17:09:49+03:00",
        "date_finish": "2026-11-03T17:09:49+03:00",
        "operating_reset_at": 1793715540,
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
[`object`](../../../data-types.md) | Option description. [Option fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Option Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Option ID ||
|| **fieldId**
[`integer`](../../../data-types.md) | List field ID ||
|| **value**
[`string`](../../../data-types.md) | Option text ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Whether the option is selected by default ||
|| **sort**
[`integer`](../../../data-types.md) | Option position in the list ||
|| **xmlId**
[`string`](../../../data-types.md) | Option external code ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Option Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check the option ID with the [crm.&lt;type&gt;.field.custom.enum.list](./crm-entity-field-custom-enum-list-rest-v3.md) method. If `id` is a string that does not consist only of digits, the error message contains `0` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the option `id` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `fields.value` | This request requires data type `non-empty string` in field `fields.value` | Pass non-empty option text ||
|| `fields.xmlId` | List element with XML_ID=`#XML_ID#` already exists | Pass a different `xmlId`: the external code is unique within the field ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Field `id` is read-only | Remove `id` from `fields`: the option ID is passed in the `id` parameter at the same level as `fields` ||
|#

#### Field Not Found

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#ID#` was not found | The option belongs to a field of a different object type. Make sure the object type in the method name matches the field type ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `EnumDto` | Check the parameter name against the [fields parameter table](#fields) ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.custom.enum.update`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company does not have these methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-custom-enum-add-rest-v3.md)
- [{#T}](./crm-entity-field-custom-enum-get-rest-v3.md)
- [{#T}](./crm-entity-field-custom-enum-list-rest-v3.md)
- [{#T}](./crm-entity-field-custom-enum-delete-rest-v3.md)
