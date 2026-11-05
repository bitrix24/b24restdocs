# Update a Custom Field v 3.0 crm.[type].field.custom.update

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

The `crm.<type>.field.custom.update` method updates the settings of a custom field and returns its description. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.custom.update` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.custom.update` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.custom.update` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.custom.update` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.custom.update` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.custom.update` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.custom.update`, for example, `crm.smartprocess1038.field.custom.update` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have custom field methods regardless of the type settings. My company does not have these methods: Bitrix24 responds to `crm.mycompany.field.custom.update` with the ["Method Not Found"](#method-not-found) error.

The method name is written in lowercase: `crm.smartinvoice.field.custom.update`, not `crm.smartInvoice.field.custom.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Field ID or field name, for example, `6008209` or `"UF_CRM_CONTRACT_NUMBER"`.

Field IDs and names are returned by the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with the new field settings in the format `{"parameter": "value"}`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

Pass only the parameters you want to change. You can change:

- `xmlId`, `sort`, `isMandatory`, `showFilter`, `showInList`, `editInList`, `isSearchable`
- `settings` — field type settings
- the `editFormLabel`, `listColumnLabel`, `listFilterLabel`, `errorMessage`, `helpMessage` labels

The values and format of the parameters are the same as in the [crm.&lt;type&gt;.field.custom.add](./crm-entity-field-custom-add-rest-v3.md#fields) method.

The field name `name`, the type `userTypeId`, and the `isMultiple` flag, which indicates whether the field accepts multiple values, cannot be changed after the field is created. For `isMultiple`, the method returns a [validation error](#dto-validation).

{% note warning "" %}

The method skips `name` and `userTypeId` without an error and changes the other parameters. If `fields` contains nothing else, the method returns the "At least one field must be specified." error.

{% endnote %}

{% note warning "" %}

The method replaces the `settings` entirely: settings that are not passed are reset to their default values. For example, if a field has `minLength` = `3` and you pass only `maxLength` in `settings`, `minLength` becomes `0`. Pass in `settings` all the settings you want to keep. The current settings are returned by the [crm.&lt;type&gt;.field.custom.get](./crm-entity-field-custom-get-rest-v3.md) method.

{% endnote %}

A string label changes only the text in the default language of Bitrix24, and an object with language codes changes the labels in the specified languages. The other labels are retained.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.custom.update`

{% endnote %}

The examples change the field label in two languages, the filter search mode, and the maximum value length of a field of the SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6008209,"fields":{"editFormLabel":{"en":"Client contract number","de":"Kundenvertragsnummer"},"showFilter":"S","settings":{"maxLength":40}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.custom.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6008209,"fields":{"editFormLabel":{"en":"Client contract number","de":"Kundenvertragsnummer"},"showFilter":"S","settings":{"maxLength":40}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.custom.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldUpdateResult = {
      item: {
        id: number
        name: string
        userTypeId: string
        editFormLabel: string | null
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CustomFieldUpdateResult>(
        'crm.smartprocess1038.field.custom.update',
        {
          id: 6008209,
          fields: {
            editFormLabel: {
              en: 'Client contract number',
              de: 'Kundenvertragsnummer',
            },
            showFilter: 'S',
            settings: {
              maxLength: 40,
            },
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field updated:', result.item.id, result.item.editFormLabel)
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
      async function updateCustomField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.custom.update',
            {
              id: 6008209,
              fields: {
                editFormLabel: {
                  en: 'Client contract number',
                  de: 'Kundenvertragsnummer',
                },
                showFilter: 'S',
                settings: {
                  maxLength: 40,
                },
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field updated:', result.item.id, result.item.editFormLabel)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCustomField)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.custom.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 6008209,
                "fields": {
                    "editFormLabel": {
                        "en": "Client contract number",
                        "de": "Kundenvertragsnummer",
                    },
                    "showFilter": "S",
                    "settings": {
                        "maxLength": 40,
                    },
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"]["id"], result["item"]["editFormLabel"])
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
                'crm.smartprocess1038.field.custom.update',
                [
                    'id' => 6008209,
                    'fields' => [
                        'editFormLabel' => [
                            'en' => 'Client contract number',
                            'de' => 'Kundenvertragsnummer',
                        ],
                        'showFilter' => 'S',
                        'settings' => [
                            'maxLength' => 40,
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.field.custom.update',
        {
            id: 6008209,
            fields: {
                editFormLabel: {
                    en: 'Client contract number',
                    de: 'Kundenvertragsnummer',
                },
                showFilter: 'S',
                settings: {
                    maxLength: 40,
                },
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
        'crm.smartprocess1038.field.custom.update',
        [
            'id' => 6008209,
            'fields' => [
                'editFormLabel' => [
                    'en' => 'Client contract number',
                    'de' => 'Kundenvertragsnummer',
                ],
                'showFilter' => 'S',
                'settings' => [
                    'maxLength' => 40,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.custom.update", b24.Params{
    	"id": 6008209,
    	"fields": b24.Params{
    		"editFormLabel": b24.Params{
    			"en": "Client contract number",
    			"de": "Kundenvertragsnummer",
    		},
    		"showFilter": "S",
    		"settings": b24.Params{
    			"maxLength": 40,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.custom.update: %w", err)
    }

    var out struct {
    	Item struct {
    		ID   int    `json:"id"`
    		Name string `json:"name"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Item.ID, out.Item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 6008209,
            "entityId": "CRM_19",
            "name": "UF_CRM_CONTRACT_NUMBER",
            "userTypeId": "string",
            "xmlId": null,
            "sort": 100,
            "isMultiple": false,
            "isMandatory": false,
            "showFilter": "S",
            "showInList": true,
            "editInList": true,
            "isSearchable": false,
            "settings": {
                "size": 20,
                "rows": 1,
                "regexp": "",
                "minLength": 0,
                "maxLength": 40,
                "defaultValue": ""
            },
            "editFormLabel": "Client contract number",
            "listColumnLabel": null,
            "listFilterLabel": null,
            "errorMessage": null,
            "helpMessage": null
        }
    },
    "time": {
        "start": 1793716015,
        "finish": 1793716015.706175,
        "duration": 0.7061750888824463,
        "processing": 0,
        "date_start": "2026-11-03T17:26:55+03:00",
        "date_finish": "2026-11-03T17:26:55+03:00",
        "operating_reset_at": 1793716560,
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
[`object`](../../../data-types.md) | Description of the field after the update: [field parameters](./crm-entity-field-custom-add-rest-v3.md#fields), the field ID `id`, and the object type code `entityId`. The type code is, for example, `CRM_DEAL` for a deal or `CRM_19` for the SPA from the examples. The number in the SPA code is the `id` field from [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md), not `entityTypeId` ||
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
                "message": "Field \"isMultiple\" is read-only.",
                "field": "isMultiple"
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the field `id` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `fields` | At least one field must be specified. | Pass at least one parameter that can be changed in `fields` ||
|| `id` | This request requires data type `integer or string starting with UF_` in field `id` | Pass a numeric field ID or a name that starts with `UF_` ||
|| `isMandatory` | This request requires data type `bool` in field `isMandatory` | Pass a value of the type specified in the parameter description. Boolean parameters accept only `true` or `false` ||
|| `settings` | This request requires data type `array` in field `settings` | Pass `settings` as an object ||
|#

#### Object Validation Errors {#dto-validation}

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `isMultiple` | Field `isMultiple` is read-only | Remove `isMultiple` from `fields`: whether the field accepts multiple values cannot be changed after creation ||
|| `id` | Field `id` is read-only | Remove `id` from `fields`: pass the field ID in the `id` parameter at the same level as `fields` ||
|| `entityId` | Field `entityId` is read-only | Remove `entityId` from `fields`: the object type is set by the method name ||
|| `showFilter` | Field value is not one of the possible values: N, I, E, S | Pass one of the values `N`, `I`, `E`, `S` ||
|#

#### Field Not Found

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#ID#` was not found | Check the field ID or name with the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method. The same error is returned if the field belongs to a different object type or is not available to the user ||
|#

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `{}` | Remove `filter` from the request: the method updates one field by `id` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `CustomFieldConfigDto` | Check the parameter name against the [fields parameter table](./crm-entity-field-custom-add-rest-v3.md#fields). If the error names the field `0`, `fields` was passed as a string or an array: pass an object ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.custom.update`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company does not have these methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-custom-add-rest-v3.md)
- [{#T}](./crm-entity-field-custom-get-rest-v3.md)
- [{#T}](./crm-entity-field-custom-list-rest-v3.md)
- [{#T}](./crm-entity-field-custom-delete-rest-v3.md)
