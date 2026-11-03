# Create a Custom Field v 3.0 crm.[type].field.custom.add

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

The `crm.<type>.field.custom.add` method creates a custom field for a CRM object type and returns the description of the created field. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.field.custom.add` | [Deal fields](../items/fields-deal.md) ||
|| Lead | `crm.lead.field.custom.add` | [Lead fields](../items/fields-lead.md) ||
|| Contact | `crm.contact.field.custom.add` | [Contact fields](../items/fields-contact.md) ||
|| Company | `crm.company.field.custom.add` | [Company fields](../items/fields-company.md) ||
|| Estimate | `crm.quote.field.custom.add` | [Estimate fields](../items/fields-quote.md) ||
|| Invoice | `crm.smartinvoice.field.custom.add` | [Invoice fields](../items/fields-smart-invoice.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.field.custom.add`, for example, `crm.smartprocess1038.field.custom.add` | [SPA fields](../items/fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. SPAs have custom field methods regardless of the type settings. My company does not have these methods: Bitrix24 responds to `crm.mycompany.field.custom.add` with the ["Method Not Found"](#method-not-found) error.

The method name is written in lowercase: `crm.smartinvoice.field.custom.add`, not `crm.smartInvoice.field.custom.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the settings of the new field in the format `{"parameter": "value"}`. The required fields are `name` and `userTypeId`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field name. The field value is passed under this name in the `fields` of item methods, for example, [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md). [Name rules](#name).

The name cannot be changed after the field is created ||
|| **userTypeId***
[`string`](../../../data-types.md) | Field type. [Possible values](#user-type-id).

The type cannot be changed after the field is created ||
|| **xmlId**
[`string`](../../../data-types.md) | External code of the field, for example, the field ID in an external system ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order of the field in the field list.

Default: `100` ||
|| **isMultiple**
[`boolean`](../../../data-types.md) | Whether the field accepts multiple values.

Possible values:

- `true` — the field stores multiple values; pass the field value of an item as an array
- `false` — the field stores a single value

Default: `false`

This attribute cannot be changed after the field is created ||
|| **isMandatory**
[`boolean`](../../../data-types.md) | Whether the field is required.

Possible values:

- `true` — the field is required
- `false` — the field is optional

Default: `false` ||
|| **showFilter**
[`string`](../../../data-types.md) | Whether to show the field in the item list filter and how to search by it.

Possible values:

- `N` — do not show in the filter
- `I` — show, search by exact match
- `E` — show, search by mask
- `S` — show, search by substring

Default: `N` ||
|| **showInList**
[`boolean`](../../../data-types.md) | Whether to show the field in the item list.

Possible values:

- `true` — show
- `false` — do not show

Default: `true` ||
|| **editInList**
[`boolean`](../../../data-types.md) | Whether the field value can be edited in the item list.

Possible values:

- `true` — allowed
- `false` — not allowed

Default: `true` ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Whether the field values are included in search.

Possible values:

- `true` — included
- `false` — not included

Default: `false` ||
|| **settings**
[`object`](../../../data-types.md) | Settings that depend on the field type, for example, the default value or the information block to link to. [Parameter description](#settings) ||
|| **editFormLabel**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) | Field label in the item card. [How to pass labels](#labels) ||
|| **listColumnLabel**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) | Header of the field column in the item list ||
|| **listFilterLabel**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) | Field label in the filter ||
|| **errorMessage**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) | Text of the error the user sees when the value is invalid ||
|| **helpMessage**
[`string`](../../../data-types.md) \| [`object`](../../../data-types.md) | Text of the field tooltip ||
|#

Parameter names are written in camelCase: `userTypeId`, `isMultiple`. The method does not accept the `id` and `entityId` fields: Bitrix24 assigns the field ID itself, and the object type is set by the method name.

#### Name Rules {#name}

- the name starts with `UF_`, for example, `UF_CRM_CONTRACT_NUMBER`
- the name contains only uppercase Latin letters, digits, and `_`
- the length is no more than 50 characters
- the name is unique within the object type

Names already in use are returned by the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method.

#### Field Types {#user-type-id}

- `string` — string
- `rich_text` — formatted text
- `integer` — integer
- `double` — decimal number
- `boolean` — yes or no
- `date` — date
- `datetime` — date and time
- `money` — money
- `url` — link
- `address` — address
- `employee` — link to an employee
- `crm` — link to CRM items
- `crm_status` — link to a CRM directory
- `iblock_section` — link to information block sections
- `iblock_element` — link to information block elements

The method does not create fields of the "List" `enumeration` and "File" `file` types and returns a [validation error](#dto-validation). The values of an existing list field are managed by the [crm.&lt;type&gt;.field.custom.enum.*](./crm-entity-field-custom-enum-list-rest-v3.md) methods.

#### Field Labels {#labels}

The `editFormLabel`, `listColumnLabel`, `listFilterLabel`, `errorMessage`, and `helpMessage` labels can be passed in two ways:

- as a string — the label is stored in the default language of Bitrix24
- as an object with language codes — the label is stored in each of the specified languages: `{"en": "Contract number", "de": "Vertragsnummer"}`

In the response, the method returns the label as a string in the default language of Bitrix24. If the label is not set in this language, the method returns `null`.

### The settings Parameter {#settings}

The `settings` keys are written in camelCase: `iblockId`, `defaultValue`, `maxLength`. This rule applies only to first-level keys: Bitrix24 does not rename the keys of nested objects. For example, for a `datetime` field, the `defaultValue` value is returned as `{"TYPE": "NONE", "VALUE": ""}`.

Each field type has its own set of keys. It is returned by the [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) method with uppercase keys: the `IBLOCK_ID` key corresponds to `iblockId`.

Boolean settings are passed as `true` or `false`. Settings that are not passed are filled in by Bitrix24 with default values and returned in the response.

For `iblock_section` and `iblock_element` fields, the `iblockId` setting is required: without it, the method returns the "Select an information block to bind to this field" error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.field.custom.add`

{% endnote %}

The examples create the "Contract number" string field for an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"UF_CRM_CONTRACT_NUMBER","userTypeId":"string","editFormLabel":{"en":"Contract number","de":"Vertragsnummer"},"showFilter":"E","settings":{"maxLength":50}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.field.custom.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"UF_CRM_CONTRACT_NUMBER","userTypeId":"string","editFormLabel":{"en":"Contract number","de":"Vertragsnummer"},"showFilter":"E","settings":{"maxLength":50}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.field.custom.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldAddResult = {
      item: {
        id: number
        name: string
        userTypeId: string
        settings: Record<string, unknown>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CustomFieldAddResult>(
        'crm.smartprocess1038.field.custom.add',
        {
          fields: {
            name: 'UF_CRM_CONTRACT_NUMBER',
            userTypeId: 'string',
            editFormLabel: {
              en: 'Contract number',
              de: 'Vertragsnummer',
            },
            showFilter: 'E',
            settings: {
              maxLength: 50,
            },
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field created:', result.item.id, result.item.name)
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
      async function addCustomField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.field.custom.add',
            {
              fields: {
                name: 'UF_CRM_CONTRACT_NUMBER',
                userTypeId: 'string',
                editFormLabel: {
                  en: 'Contract number',
                  de: 'Vertragsnummer',
                },
                showFilter: 'E',
                settings: {
                  maxLength: 50,
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
          console.info('Field created:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCustomField)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.field.custom.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "name": "UF_CRM_CONTRACT_NUMBER",
                    "userTypeId": "string",
                    "editFormLabel": {
                        "en": "Contract number",
                        "de": "Vertragsnummer",
                    },
                    "showFilter": "E",
                    "settings": {
                        "maxLength": 50,
                    },
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"]["id"], result["item"]["name"])
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
                'crm.smartprocess1038.field.custom.add',
                [
                    'fields' => [
                        'name' => 'UF_CRM_CONTRACT_NUMBER',
                        'userTypeId' => 'string',
                        'editFormLabel' => [
                            'en' => 'Contract number',
                            'de' => 'Vertragsnummer',
                        ],
                        'showFilter' => 'E',
                        'settings' => [
                            'maxLength' => 50,
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
        'crm.smartprocess1038.field.custom.add',
        {
            fields: {
                name: 'UF_CRM_CONTRACT_NUMBER',
                userTypeId: 'string',
                editFormLabel: {
                    en: 'Contract number',
                    de: 'Vertragsnummer',
                },
                showFilter: 'E',
                settings: {
                    maxLength: 50,
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
        'crm.smartprocess1038.field.custom.add',
        [
            'fields' => [
                'name' => 'UF_CRM_CONTRACT_NUMBER',
                'userTypeId' => 'string',
                'editFormLabel' => [
                    'en' => 'Contract number',
                    'de' => 'Vertragsnummer',
                ],
                'showFilter' => 'E',
                'settings' => [
                    'maxLength' => 50,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.field.custom.add", b24.Params{
    	"fields": b24.Params{
    		"name":       "UF_CRM_CONTRACT_NUMBER",
    		"userTypeId": "string",
    		"editFormLabel": b24.Params{
    			"en": "Contract number",
    			"de": "Vertragsnummer",
    		},
    		"showFilter": "E",
    		"settings": b24.Params{
    			"maxLength": 50,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.field.custom.add: %w", err)
    }

    var created struct {
    	Item struct {
    		ID   int    `json:"id"`
    		Name string `json:"name"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &created); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(created.Item.ID, created.Item.Name)
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
            "showFilter": "E",
            "showInList": true,
            "editInList": true,
            "isSearchable": false,
            "settings": {
                "size": 20,
                "rows": 1,
                "regexp": "",
                "minLength": 0,
                "maxLength": 50,
                "defaultValue": ""
            },
            "editFormLabel": "Contract number",
            "listColumnLabel": null,
            "listFilterLabel": null,
            "errorMessage": null,
            "helpMessage": null
        }
    },
    "time": {
        "start": 1793716013,
        "finish": 1793716014.048628,
        "duration": 1.0486280918121338,
        "processing": 1,
        "date_start": "2026-11-03T17:26:53+03:00",
        "date_finish": "2026-11-03T17:26:54+03:00",
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
[`object`](../../../data-types.md) | Description of the created field: all [field parameters](#fields), including those not passed in the request, with default values. In addition, the object contains two keys:

- `id` — field ID. It is required for the [crm.&lt;type&gt;.field.custom.update](./crm-entity-field-custom-update-rest-v3.md), [crm.&lt;type&gt;.field.custom.get](./crm-entity-field-custom-get-rest-v3.md), and [crm.&lt;type&gt;.field.custom.delete](./crm-entity-field-custom-delete-rest-v3.md) methods
- `entityId` — object type code: `CRM_DEAL` for a deal, `CRM_19` for the SPA from the examples. The number in the SPA code is the `id` field from [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md), not `entityTypeId` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION",
        "message": "Record with ID UF_CRM_CONTRACT_NUMBER already exists."
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
|| `isMultiple` | This request requires data type `bool` in field `isMultiple` | Pass a value of the type specified in the parameter description. Boolean parameters accept only `true` or `false` ||
|| `settings` | This request requires data type `array` in field `settings` | Pass `settings` as an object ||
|| `name` | The field name is required. | Pass a non-empty field name in `name` ||
|| `field` | The field name prefix is not UF_ | Start the name with `UF_`, for example, `UF_CRM_CONTRACT_NUMBER` ||
|| `field` | The field name contains invalid symbols. Valid symbols are: A-Z, 0-9 and _. | Use only uppercase Latin letters, digits, and `_` in the name ||
|| `field` | The field name is too long (exceeds 50 symbols). | Shorten the name to 50 characters ||
|| `field` | Select an information block to bind to this field | Pass `iblockId` in `settings` for an `iblock_section` or `iblock_element` field ||
|#

Errors in the field name and information block settings are returned with `"field": "field"`, and their text ends with the `<br>` tag. If there are several such errors, Bitrix24 combines them into one message.

#### Object Validation Errors {#dto-validation}

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `userTypeId` | Required field `userTypeId` is empty | Pass `userTypeId` in `fields` ||
|| `userTypeId` | Field value is not one of the possible values: `#VALUES#` | Pass a type from the [list of types](#user-type-id). The method does not create `enumeration` and `file` fields ||
|| `showFilter` | Field value is not one of the possible values: N, I, E, S | Pass one of the values `N`, `I`, `E`, `S` ||
|| `id` | Field `id` is read-only | Remove `id` from `fields`: the ID is assigned automatically ||
|| `entityId` | Field `entityId` is read-only | Remove `entityId` from `fields`: the object type is set by the method name ||
|#

#### Field Already Exists

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `#NAME#` already exists | Choose a different field name. Names already in use are returned by the [crm.&lt;type&gt;.field.custom.list](./crm-entity-field-custom-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `CustomFieldConfigDto` | Check the parameter name against the [fields parameter table](#fields). An error with the field name `0` occurs when `fields` is passed as a string or an array instead of an object ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.field.custom.add`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company has no custom field methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-field-custom-update-rest-v3.md)
- [{#T}](./crm-entity-field-custom-get-rest-v3.md)
- [{#T}](./crm-entity-field-custom-list-rest-v3.md)
- [{#T}](./crm-entity-field-custom-delete-rest-v3.md)
- [{#T}](./crm-entity-field-required-set-rest-v3.md)
