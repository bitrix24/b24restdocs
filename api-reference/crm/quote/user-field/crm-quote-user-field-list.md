# Get a List of Custom Fields For crm.quote.userfield.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: User with read access to estimates

The method `crm.quote.userfield.list` returns a list of custom fields for estimates based on a filter: field names, types, and settings.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the field to filter by
- `value_n` — the filter value

If `filter` is not provided, the method returns all custom estimate fields.

A field is included in the response if it matches all conditions at once (`AND`). The filter supports exact match only. The method silently skips conditions with the `>`, `<`, `!`, `%` prefixes and conditions with an array of values. See the [list of available fields for filtering](#filterable) below. ||
|| **order**
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the field to sort by
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

Available fields for sorting:
- `ID` — identifier of the custom field
- `FIELD_NAME` — full field name with the `UF_CRM_` prefix
- `USER_TYPE_ID` — type of the custom field
- `XML_ID` — external code
- `SORT` — sort index

By default:
```
{
    "SORT": "ASC",
    "ID": "ASC"
}
```
||
|#

### Available Fields for Filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the custom field ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Full field name with the `UF_CRM_` prefix ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Type of the custom field. Possible values:
- `string` — string
- `integer` — integer
- `double` — number
- `boolean` — yes/no
- `datetime` — date/time
- `date` — date
- `money` — money
- `url` — link
- `address` — address
- `enumeration` — list
- `file` — file
- `employee` — binding to an employee
- `crm_status` — binding to the CRM directory
- `iblock_section` — binding to information block sections
- `iblock_element` — binding to information block elements
- `crm` — binding to CRM elements
- [custom field types](../../universal/user-defined-fields/userfield-type.md)

The method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) returns the full list of types available in Bitrix24 ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting index ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Is the custom field multiple.
Possible values:
- `Y` — yes
- `N` — no ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Is the custom field mandatory. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Field mode in the list filter:
- `N` — the field is not in the filter
- `I` — exact match search
- `E` — mask search
- `S` — substring search ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show the field in the list. Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no
||
|| **LANG**
[`string`](../../../data-types.md) | [Language identifier](../../data-types.md#lang-ids). If you pass `LANG`, the method returns the field labels in this language:
- `EDIT_FORM_LABEL` — label in the edit form
- `LIST_COLUMN_LABEL` — header in the list
- `LIST_FILTER_LABEL` — filter label in the list
- `ERROR_MESSAGE` — error message
- `HELP_MESSAGE` — help ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example retrieves fields that are both multiple and mandatory. `LANG` in the filter adds field labels in German to the response.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.userfield.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type QuoteUserFieldItem = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: string
      MANDATORY: string
      SHOW_FILTER: string
      SHOW_IN_LIST: string
      EDIT_IN_LIST: string
      IS_SEARCHABLE: string
      SETTINGS: Record<string, unknown>
      EDIT_FORM_LABEL: string | null
      LIST_COLUMN_LABEL: string | null
      LIST_FILTER_LABEL: string | null
      ERROR_MESSAGE: string | null
      HELP_MESSAGE: string | null
    }

    try {
      // crm.quote.userfield.list returns all matching fields in one response (no paging)
      const response = await $b24.actions.v2.call.make<QuoteUserFieldItem[]>({
        method: 'crm.quote.userfield.list',
        params: {
          filter: {
            MULTIPLE: 'Y',
            MANDATORY: 'Y',
            LANG: 'de',
          },
          order: {
            USER_TYPE_ID: 'ASC',
            SORT: 'ASC',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User fields count:', result.length, 'fields:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchQuoteUserFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.quote.userfield.list returns all matching fields in one response (no paging)
          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.userfield.list',
            params: {
              filter: {
                MULTIPLE: 'Y',
                MANDATORY: 'Y',
                LANG: 'de',
              },
              order: {
                USER_TYPE_ID: 'ASC',
                SORT: 'ASC',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User fields count:', result.length, 'fields:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchQuoteUserFields)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.quote.userfield.list(
            filter={"MULTIPLE": "Y", "MANDATORY": "Y", "LANG": "de"},
            order={"USER_TYPE_ID": "ASC", "SORT": "ASC"},
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list`

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.quote.userfield.list(
            filter={"MULTIPLE": "Y", "MANDATORY": "Y", "LANG": "de"},
            order={"USER_TYPE_ID": "ASC", "SORT": "ASC"},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.userfield.list',
                [
                    'filter' => [
                        'MULTIPLE' => 'Y',
                        'MANDATORY' => 'Y',
                        'LANG' => 'de',
                    ],
                    'order' => [
                        'USER_TYPE_ID' => 'ASC',
                        'SORT' => 'ASC',
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

    ```js
    BX24.callMethod(
        'crm.quote.userfield.list',
        {
            filter: {
                MULTIPLE: "Y",
                MANDATORY: "Y",
                LANG: "de",
            },
            order: {
                USER_TYPE_ID: "ASC",
                SORT: "ASC",
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.userfield.list',
        [
            'filter' => [
                'MULTIPLE' => "Y",
                'MANDATORY' => "Y",
                'LANG' => "de",
            ],
            'order' => [
                'USER_TYPE_ID' => "ASC",
                'SORT' => "ASC",
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.userfield.list", b24.Params{
    	"filter": b24.Params{
    		"MULTIPLE":  "Y",
    		"MANDATORY": "Y",
    		"LANG":      "ru",
    	},
    	"order": b24.Params{
    		"USER_TYPE_ID": "ASC",
    		"SORT":         "ASC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.quote.userfield.list: %w", err)
    }

    var items []struct {
    	ID         b24.ID `json:"ID"`
    	EntityID   string `json:"ENTITY_ID"`
    	FieldName  string `json:"FIELD_NAME"`
    	UserTypeID string `json:"USER_TYPE_ID"`
    	Sort       string `json:"SORT"`
    	Multiple   string `json:"MULTIPLE"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.EntityID)
    }

    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "5815",
            "ENTITY_ID": "CRM_QUOTE",
            "FIELD_NAME": "UF_CRM_1713790573",
            "USER_TYPE_ID": "crm_status",
            "XML_ID": null,
            "SORT": "100",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "I",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "ENTITY_TYPE": "INDUSTRY"
            },
            "EDIT_FORM_LABEL": "Directory",
            "LIST_COLUMN_LABEL": "Directory",
            "LIST_FILTER_LABEL": "Directory",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6799",
            "ENTITY_ID": "CRM_QUOTE",
            "FIELD_NAME": "UF_CRM_1724077760",
            "USER_TYPE_ID": "enumeration",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DISPLAY": "LIST",
                "LIST_HEIGHT": 1,
                "CAPTION_NO_VALUE": "",
                "SHOW_NO_VALUE": "Y"
            },
            "EDIT_FORM_LABEL": "Radio",
            "LIST_COLUMN_LABEL": "Radio",
            "LIST_FILTER_LABEL": "Radio",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null,
            "LIST": [
                {
                    "ID": "3157",
                    "SORT": "10",
                    "VALUE": "Kids Radio",
                    "DEF": "N",
                    "XML_ID": "79b4c576f96e65eb40f390e45c0dc802"
                },
                {
                    "ID": "3159",
                    "SORT": "20",
                    "VALUE": "Chanson Radio",
                    "DEF": "N",
                    "XML_ID": "d3ffd89a825f218f5efd79dffd38fbbf"
                },
                {
                    "ID": "3161",
                    "SORT": "30",
                    "VALUE": "Love Radio",
                    "DEF": "N",
                    "XML_ID": "dff769fa19d7d7ce8d0677341d221161"
                },
                {
                    "ID": "3163",
                    "SORT": "40",
                    "VALUE": "Monte Carlo Radio",
                    "DEF": "N",
                    "XML_ID": "1f22e5c818ce336a42bd0ec94eb69b9e"
                },
                {
                    "ID": "3181",
                    "SORT": "130",
                    "VALUE": "DFM Yuryev-Polsky",
                    "DEF": "N",
                    "XML_ID": "fc1c5e6b4a9fd20b4749240b8dbac41a"
                }
            ]
        },
        {
            "ID": "6791",
            "ENTITY_ID": "CRM_QUOTE",
            "FIELD_NAME": "UF_CRM_1722578010",
            "USER_TYPE_ID": "file",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "SIZE": 20,
                "LIST_WIDTH": 0,
                "LIST_HEIGHT": 0,
                "MAX_SHOW_SIZE": 0,
                "MAX_ALLOWED_SIZE": 0,
                "EXTENSIONS": [],
                "TARGET_BLANK": "Y"
            },
            "EDIT_FORM_LABEL": "KP (files)",
            "LIST_COLUMN_LABEL": "KP (files)",
            "LIST_FILTER_LABEL": "KP (files)",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "5567",
            "ENTITY_ID": "CRM_QUOTE",
            "FIELD_NAME": "UF_CRM_1709565075",
            "USER_TYPE_ID": "resourcebooking",
            "XML_ID": null,
            "SORT": "1",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "USE_USERS": "N",
                "USE_RESOURCES": "Y",
                "RESOURCES": {
                    "resource": {
                        "XML_ID": "resource",
                        "NAME": "resource",
                        "SECTIONS": [
                            {
                                "ID": "7",
                                "CAL_TYPE": "resource",
                                "NAME": "Museum Hall"
                            },
                            {
                                "ID": "49",
                                "CAL_TYPE": "resource",
                                "NAME": "resource 1"
                            },
                            {
                                "ID": "51",
                                "CAL_TYPE": "resource",
                                "NAME": "resource 2"
                            },
                            {
                                "ID": "53",
                                "CAL_TYPE": "resource",
                                "NAME": "1"
                            },
                            {
                                "ID": "57",
                                "CAL_TYPE": "resource",
                                "NAME": "Resource"
                            },
                            {
                                "ID": "59",
                                "CAL_TYPE": "resource",
                                "NAME": "Resource 2"
                            }
                        ]
                    }
                },
                "SELECTED_RESOURCES": [
                    {
                        "type": "resource",
                        "id": "7",
                        "title": "Museum Hall"
                    }
                ],
                "SELECTED_USERS": [],
                "FULL_DAY": "N",
                "ALLOW_OVERBOOKING": "Y",
                "USE_SERVICES": "N",
                "SERVICE_LIST": [
                    {
                        "name": "",
                        "duration": "60"
                    }
                ],
                "RESOURCE_LIMIT": -1,
                "TIMEZONE": "Europe/Berlin",
                "USE_USER_TIMEZONE": "N"
            },
            "EDIT_FORM_LABEL": "BOOK MEETING",
            "LIST_COLUMN_LABEL": "BOOK MEETING",
            "LIST_FILTER_LABEL": "BOOK MEETING",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6997",
            "ENTITY_ID": "CRM_QUOTE",
            "FIELD_NAME": "UF_CRM_HELLO_WORLD",
            "USER_TYPE_ID": "string",
            "XML_ID": null,
            "SORT": "2000",
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "N",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "SIZE": 20,
                "ROWS": 10,
                "REGEXP": "",
                "MIN_LENGTH": 0,
                "MAX_LENGTH": 0,
                "DEFAULT_VALUE": "Hello, world! Default value (changed)"
            },
            "EDIT_FORM_LABEL": "Hello, world! Edit (changed)",
            "LIST_COLUMN_LABEL": "Hello, world! Column (changed)",
            "LIST_FILTER_LABEL": "Hello, world! Filter (changed)",
            "ERROR_MESSAGE": "Hello, world! Error (changed)",
            "HELP_MESSAGE": "Hello, world! Help (changed)"
        }
    ],
    "total": 5,
    "time": {
        "start": 1753793143.219832,
        "finish": 1753793143.529472,
        "duration": 0.30964016914367676,
        "processing": 0.06361007690429688,
        "date_start": "2025-07-29T15:45:43+03:00",
        "date_finish": "2025-07-29T15:45:43+03:00",
        "operating_reset_at": 1753793743,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Array of custom estimate fields [(detailed description)](#result). If no fields match, an empty array `[]`.

The method has no pagination: all matching fields come in one response, and the `start` parameter is ignored ||
|| **total**
[`integer`](../../../data-types.md) | Number of found custom fields. Equals the length of the `result` array ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### result Array Element {#result}

Numeric values of `ID` and `SORT` are returned as strings.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | Custom field identifier. Used in the methods [crm.quote.userfield.get](./crm-quote-user-field-get.md), [crm.quote.userfield.update](./crm-quote-user-field-update.md), and [crm.quote.userfield.delete](./crm-quote-user-field-delete.md) ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | The object the field belongs to. For estimates, always `CRM_QUOTE` ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Full field name with the `UF_CRM_` prefix. The universal methods [crm.item.update](../../universal/crm-item-update.md) and [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 7` write and read the field value. In these methods, the field name is written in camelCase: `UF_CRM_MANAGER_NOTE` becomes `ufCrmManagerNote`. The methods [crm.quote.update](../crm-quote-update.md) and [crm.quote.get](../crm-quote-get.md) work with the full name, but their development has been halted. The method `crm.quote.userfield.list` does not return field values in estimates ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Field type; for values, see the [list of fields for filtering](#filterable) ||
|| **USER_TYPE_OWNER**
[`string`](../../../data-types.md) | Identifier of the application that registered the [custom field type](../../universal/user-defined-fields/userfield-type.md). Returned only for fields of a custom type ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code. If not set, `null` ||
|| **SORT**
[`string`](../../../data-types.md) | Sort index ||
|| **MULTIPLE**, **MANDATORY**, **SHOW_IN_LIST**, **EDIT_IN_LIST**, **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Field flags with the values `Y` or `N`; for their meaning, see the [list of fields for filtering](#filterable) ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Filter mode: `N`, `I`, `E`, or `S`; for their meanings, see the [list of fields for filtering](#filterable) ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Field type settings, for example, `DEFAULT_VALUE` or `PRECISION`. The set of keys depends on `USER_TYPE_ID`. The method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) returns the list of keys for a type. The default value of `datetime` fields is returned in ISO 8601 format ||
|| **EDIT_FORM_LABEL**, **LIST_COLUMN_LABEL**, **LIST_FILTER_LABEL**, **ERROR_MESSAGE**, **HELP_MESSAGE**
[`string`](../../../data-types.md) | Field labels in the language from `filter.LANG`. Returned only if `LANG` is passed in the filter. If there is no label in this language, `null`.

The method [crm.quote.userfield.get](./crm-quote-user-field-get.md) returns labels in all languages at once ||
|| **LIST**
[`object[]`](../../../data-types.md) | List items. Returned only for `enumeration` fields, sorted by `SORT`. Each item contains:
- `ID` — item identifier; pass it as the field value when writing to an estimate
- `SORT` — sort index
- `VALUE` — item value
- `DEF` — whether the item is the default: `Y` or `N`
- `XML_ID` — item external code ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'filter' must be array."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Parameter 'order' must be array.` | The provided `order` is not an object ||
|| `400` | Empty value | `Parameter 'filter' must be array.` | The provided `filter` is not an object ||
|| `400` | Empty value | `Access denied.` | The user does not have read access to estimates ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-user-field-add.md)
- [{#T}](./crm-quote-user-field-update.md)
- [{#T}](./crm-quote-user-field-get.md)
- [{#T}](./crm-quote-user-field-delete.md)
- [{#T}](../../universal/crm-item-get.md)
- [{#T}](../../universal/crm-item-update.md)
- [{#T}](./index.md)
