# Get a List of Custom Fields for Leads crm.lead.userfield.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with read access to leads

The method `crm.lead.userfield.list` returns a list of custom fields for leads based on a filter.

## Method Parameters

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

All conditions for individual fields are combined using `AND`. See the [list of available fields for filtering](#filterable) below. ||
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

Available fields for sorting:
- `ID` — identifier of the custom field
- `FIELD_NAME` — code of the custom field
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

### Filterable Fields {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the custom field ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Code of the custom field ||
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
||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index ||
|| **MULTIPLE**
[`boolean`](../../../data-types.md) | Indicates whether the custom field is multiple.
Possible values:
- `Y` — yes
- `N` — no ||
|| **MANDATORY**
[`boolean`](../../../data-types.md) | Indicates whether the custom field is mandatory. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Whether to show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring ||
|| **SHOW_IN_LIST**
[`boolean`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`boolean`](../../../data-types.md) | Whether to allow user editing. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_SEARCHABLE**
[`boolean`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no ||
|| **LANG**
[`string`](../../../data-types.md) | [Language identifier](../../data-types.md#lang-ids). When filtering by this parameter, a set of fields with values in the specified language will be provided:
- `EDIT_FORM_LABEL` — label in the edit form
- `LIST_COLUMN_LABEL` — header in the list
- `LIST_FILTER_LABEL` — filter label in the list
- `ERROR_MESSAGE` — error message
- `HELP_MESSAGE` — help ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

Get a list of custom fields that:
- are multiple,
- are mandatory,
- have custom field labels in German. The filter by the `LANG` parameter will additionally provide the field names in the response.

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

    // Shape of each user field returned in result[] (with filter.LANG the label fields are strings)
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
      EDIT_FORM_LABEL: string
      LIST_COLUMN_LABEL: string
    }

    try {
      // crm.lead.userfield.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CrmLeadUserFieldListItem[]>({
        method: 'crm.lead.userfield.list',
        params: {
          filter: {
            MULTIPLE: 'Y',
            MANDATORY: 'Y',
            LANG: 'ru',
          },
          order: {
            USER_TYPE_ID: 'ASC',
            SORT: 'ASC',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User fields on this page:', result.length, result)
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

          // crm.lead.userfield.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.lead.userfield.list',
            params: {
              filter: {
                MULTIPLE: 'Y',
                MANDATORY: 'Y',
                LANG: 'ru',
              },
              order: {
                USER_TYPE_ID: 'ASC',
                SORT: 'ASC',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User fields on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listLeadUserFields)
    </script>
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
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching company user fields: ' . $e->getMessage();
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
                'MANDATORY' => "N",
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

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.lead.userfield.list(
            filter={"MANDATORY": "N", "SHOW_IN_LIST": "Y"},
            order={"SORT": "ASC", "ID": "DESC"},
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.lead.userfield.list(
            filter={"SHOW_IN_LIST": "Y"},
            order={"SORT": "ASC"},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list_fast`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.lead.userfield.list(
            filter={"SHOW_IN_LIST": "Y"},
            order={"ID": "DESC"},
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "5815",
            "ENTITY_ID": "CRM_LEAD",
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
            "ENTITY_ID": "CRM_LEAD",
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
                    "VALUE": "DFM Yuriev-Polsky",
                    "DEF": "N",
                    "XML_ID": "fc1c5e6b4a9fd20b4749240b8dbac41a"
                }
            ]
        },
        {
            "ID": "6791",
            "ENTITY_ID": "CRM_LEAD",
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
            "EDIT_FORM_LABEL": "Estimate (files)",
            "LIST_COLUMN_LABEL": "Estimate (files)",
            "LIST_FILTER_LABEL": "Estimate (files)",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "5567",
            "ENTITY_ID": "CRM_LEAD",
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
            "EDIT_FORM_LABEL": "MEETING ROOM BOOKING",
            "LIST_COLUMN_LABEL": "MEETING ROOM BOOKING",
            "LIST_FILTER_LABEL": "MEETING ROOM BOOKING",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6997",
            "ENTITY_ID": "CRM_LEAD",
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
                "DEFAULT_VALUE": "Hello, world! Default value (modified)"
            },
            "EDIT_FORM_LABEL": "Hello, world! Edit (modified)",
            "LIST_COLUMN_LABEL": "Hello, world! Column (modified)",
            "LIST_FILTER_LABEL": "Hello, world! Filter (modified)",
            "ERROR_MESSAGE": "Hello, world! Error (modified)",
            "HELP_MESSAGE": "Hello, world! Help (modified)"
        }
    ],
    "total": 5,
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
[`object`](../../../data-types.md) | Root element of the response, contains a list of custom fields.

The structure of an individual custom field depends on its type. Fields `EDIT_FORM_LABEL`, `LIST_COLUMN_LABEL`, `LIST_FILTER_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` are returned as `string` when `filter.LANG` is provided. ||
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
|| **Code** | **Description** | **Value** ||
|| `400`     | Parameter 'order' must be array | The provided `order` is not an object ||
|| `400`     | Parameter 'filter' must be array | The provided `filter` is not an object ||
|#
{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-userfield-add.md)
- [{#T}](./crm-lead-userfield-update.md)
- [{#T}](./crm-lead-userfield-get.md)
- [{#T}](./crm-lead-userfield-delete.md)