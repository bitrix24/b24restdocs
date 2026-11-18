# Get a List of Custom Fields for Leads crm.lead.userfield.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with read access to leads

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.lead.field.custom.list](../../rest-v3/fields/crm-entity-field-custom-list-rest-v3.md).

{% endnote %}

The method `crm.lead.userfield.list` returns a list of custom fields for leads based on a filter.

The method returns all fields that match the filter in a single response, without pagination. The `start` parameter is ignored, and `next` is not returned.

## Method Parameters

All parameters are optional. Without `filter`, the method returns all custom fields of leads.

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

- `field_n` — the name of the field by which the selection of custom fields will be filtered
- `value_n` — the filter value

Only exact match is supported. Conditions are combined using `AND`. Keys with the `>`, `!`, `%` prefixes, unknown keys, array values, and empty strings are ignored. See the [list of available fields for filtering](#filterable) below. ||
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

- `field_n` — the name of the field by which the selection of elements will be sorted
- `value_n` — a `string` value that can be:
    - `ASC` — ascending order
    - `DESC` — descending order

The direction is case-insensitive. If a value other than `ASC` is passed, `DESC` is used.

Available fields for sorting:
- `ID` — identifier of the custom field
- `ENTITY_ID` — field object: `CRM_LEAD`
- `FIELD_NAME` — code of the custom field
- `USER_TYPE_ID` — type of the custom field
- `XML_ID` — external code
- `SORT` — sort index

If `order` is not set or contains no available sort fields, the following order is used:
```
{
    "SORT": "ASC",
    "ID": "ASC"
}
```
||
|#

### Filterable Fields {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the custom field ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Code of the custom field ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Field type identifier. The available types are returned by [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Indicates whether the custom field is multiple.
Possible values:
- `Y` — yes
- `N` — no ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Indicates whether the custom field is mandatory. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Whether to show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Whether to allow user editing. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no ||
|| **LANG**
[`string`](../../../data-types.md) | [Language identifier](../../data-types.md#lang-ids). Selects the language of labels and does not filter fields by the presence of a translation. When `LANG` is passed, the response additionally contains labels in the specified language (strings or `null`):
- `EDIT_FORM_LABEL` — label in the edit form
- `LIST_COLUMN_LABEL` — header in the list
- `LIST_FILTER_LABEL` — filter label in the list
- `ERROR_MESSAGE` — error message
- `HELP_MESSAGE` — help ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

Retrieve multiple required fields with labels in German. Sort them by type and sort index.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.userfield.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each user field returned in result[] (filter.LANG adds translated labels)
    type CrmLeadUserFieldListItem = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: string
      MANDATORY: string
      SHOW_IN_LIST: string
      EDIT_FORM_LABEL: string | null
      LIST_COLUMN_LABEL: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmLeadUserFieldListItem[]>({
        method: 'crm.lead.userfield.list',
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
        console.info('User fields:', result.length, result)
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
      async function listLeadUserFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.lead.userfield.list',
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
          console.info('User fields:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listLeadUserFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException
    try:
        bitrix_response = client.crm.lead.userfield.list(filter={
            "MANDATORY": "Y",
            "MULTIPLE": "Y",
            "LANG": "de",
        }, order={
            "USER_TYPE_ID": "ASC",
            "SORT": "ASC",
        }).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print('Bitrix API error', f'error: {error.error}', f'error_description: {error.error_description}', sep='\n')
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```


- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.lead.userfield.list',
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
        'crm.lead.userfield.list',
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
        'crm.lead.userfield.list',
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
    res, err := client.Core().Call(ctx, "crm.lead.userfield.list", b24.Params{
    	"filter": b24.Params{
    		"MULTIPLE":  "Y",
    		"MANDATORY": "Y",
    		"LANG":      "de",
    	},
    	"order": b24.Params{
    		"USER_TYPE_ID": "ASC",
    		"SORT":         "ASC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.lead.userfield.list: %w", err)
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

    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "6799",
            "ENTITY_ID": "CRM_LEAD",
            "FIELD_NAME": "UF_CRM_1724077760",
            "USER_TYPE_ID": "enumeration",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "Y",
            "MANDATORY": "Y",
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
                    "VALUE": "Children's Radio",
                    "DEF": "N",
                    "XML_ID": "79b4c576f96e65eb40f390e45c0dc802"
                },
                {
                    "ID": "3159",
                    "SORT": "20",
                    "VALUE": "Shanson Radio",
                    "DEF": "N",
                    "XML_ID": "d3ffd89a825f218f5efd79dffd38fbbf"
                }
            ]
        }
    ],
    "total": 1,
    "time": {
        "start": 1753793143.219832,
        "finish": 1753793143.529472,
        "duration": 0.30964016914367676,
        "processing": 0.06361007690429688,
        "date_start": "2025-07-29T15:45:43+02:00",
        "date_finish": "2025-07-29T15:45:43+02:00",
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
[`object[]`](../../../data-types.md) | Array of field descriptions. The main characteristics are listed in the [crm.lead.userfield.get response table](./crm-lead-userfield-get.md#result). Without `filter.LANG`, language labels are missing; with `filter.LANG`, they are strings or `null`, not objects. For the `enumeration` type, `LIST` items additionally contain `XML_ID` — the external code of the option ||
|| **total**
[`integer`](../../../data-types.md) | Number of found custom fields ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'filter' must be array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Parameter 'order' must be array` | The provided `order` is not an object or an array ||
|| `400` | Empty value | `Parameter 'filter' must be array` | The provided `filter` is not an object or an array ||
|| `400` | Empty value | `Access denied` | No permission to read leads ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-userfield-add.md)
- [{#T}](./crm-lead-userfield-update.md)
- [{#T}](./crm-lead-userfield-get.md)
- [{#T}](./crm-lead-userfield-delete.md)
