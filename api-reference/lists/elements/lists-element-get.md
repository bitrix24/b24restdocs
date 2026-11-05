# Get Element Parameters or List of Elements lists.element.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required list

The method `lists.element.get` returns an element or a list of elements.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Information block type ID.

Standard types:
- `lists` — universal lists
- `bitrix_processes` — processes
- `lists_socnet` — group lists

In the on-premise version, you can also specify the ID of any existing custom information block type. `IBLOCK_ID` or `IBLOCK_CODE` must point to an information block of this type. Access to the operation is determined by the user's standard permissions for the information block, section, or element ||
|| **IBLOCK_ID***
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|| **ELEMENT_ID**
[`integer`](../../data-types.md) | Identifier of the element.

The identifier can be obtained using the [lists.element.get](./lists-element-get.md) method ||
|| **ELEMENT_CODE**
[`string`](../../data-types.md) | Symbolic code of the element.

The code can be obtained using the [lists.element.get](./lists-element-get.md) method

{% note info "" %}

To retrieve element data, at least one of the parameters must be specified: `ELEMENT_ID` or `ELEMENT_CODE`

{% endnote %} ||
|| **SELECT**
[`array`](../../data-types.md) | An array containing a list of fields to select. If no fields are specified, all available fields are returned by default.

Available fields:
- `ID` — element identifier
- `CODE` — element code
- `NAME` — element name
- `IBLOCK_SECTION_ID` — identifier of the section to which the element is added
- `CREATED_BY` — identifier of the user who created the element
- `CREATED_USER_NAME` — name of the user who created the element (deprecated)
- `ACTIVE_TO` — end date of activity (deprecated)
- `BP_PUBLISHED` — publication within the business process (deprecated)
- `DATE_CREATE` — element creation date
- `PREVIEW_TEXT` — preview text (deprecated)
- `DETAIL_TEXT` — detailed text (deprecated)
- `SORT` — sorting
- `PREVIEW_TEXT_TYPE` — type of preview text (deprecated)
- `DETAIL_TEXT_TYPE` — type of detailed text (deprecated)
- `PROPERTY_PropertyId` — custom properties

  The property identifier can be obtained using the [lists.field.get](../fields/lists-field-get.md) method.

The `ID` field is always returned. The method skips fields and properties that are not in the list without an error
|| 
|| **FILTER**
[`object`] | An object for filtering element fields in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. The filterable field can take the following values: 

- `NAME` — element name
- `IBLOCK_SECTION_ID` — identifier of the section to which the element is added
- `CREATED_BY` — identifier of the user who created the element
- `ACTIVE_TO` — end date of activity (deprecated)
- `BP_PUBLISHED` — publication within the business process (deprecated)
- `DATE_CREATE` — element creation date
- `PREVIEW_TEXT` — preview text (deprecated)
- `DETAIL_TEXT` — detailed text (deprecated)
- `SORT` — sorting
- `PREVIEW_TEXT_TYPE` — type of preview text (deprecated)
- `DETAIL_TEXT_TYPE` — type of detailed text (deprecated)
- `PROPERTY_PropertyId` — custom properties

  The property identifier can be obtained using the [lists.field.get](../fields/lists-field-get.md) method.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `=` — equal, exact match. Used by default
- `!=`, `!` — not equal
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position

If no element identifier is provided and no filtering conditions are specified, all elements of the list will be returned
||
|| **ELEMENT_ORDER**
[`object`](../../data-types.md) | An object for sorting element fields in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending
  
Sorting of multiple properties, as well as properties of the following types, is not supported:
- `S:Money` — money type
- `PREVIEW_TEXT`
- `DETAIL_TEXT`
- `S:ECrm` — binding to CRM elements
- `S:map_yandex` — binding to a maps service
- `S:DiskFile` — File type (Drive)
- `IBLOCK_SECTION_ID` ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The result page size is always static — 50 records.

To select the second page of results, you must pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_ID":6999,"SELECT":["ID","CODE","NAME","IBLOCK_SECTION_ID","DATE_CREATE","PROPERTY_951","PROPERTY_1003"],"FILTER":{"NAME":"%Test%","<=DATE_CREATE":"2025-12-31",">=DATE_CREATE":"2025-01-01"},"ELEMENT_ORDER":{"NAME":"asc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_ID":6999,"SELECT":["ID","CODE","NAME","IBLOCK_SECTION_ID","DATE_CREATE","PROPERTY_951","PROPERTY_1003"],"FILTER":{"NAME":"%Test%","<=DATE_CREATE":"2025-12-31",">=DATE_CREATE":"2025-01-01"},"ELEMENT_ORDER":{"NAME":"asc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each element returned in result[]
    type ElementData = {
      ID: string
      NAME: string
      CODE: string
      IBLOCK_SECTION_ID: string | null
      CREATED_BY: string
      PROPERTY_951: Record<string, string>
      PROPERTY_1003: Record<string, string>
    }

    try {
      // lists.element.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ElementData[]>({
        method: 'lists.element.get',
        params: {
          IBLOCK_TYPE_ID: 'lists',
          IBLOCK_ID: 47,
          ELEMENT_ID: 6999,
          SELECT: [
            'ID',
            'CODE',
            'NAME',
            'IBLOCK_SECTION_ID',
            'DATE_CREATE',
            'PROPERTY_951',
            'PROPERTY_1003',
          ],
          FILTER: {
            NAME: '%Test%',
            '>=DATE_CREATE': '2025-01-01',
            '<=DATE_CREATE': '2025-12-31',
          },
          ELEMENT_ORDER: {
            NAME: 'asc',
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
        console.info('Fetched elements:', result.length, result[0]?.ID, result[0]?.NAME)
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
      async function getListElements() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // lists.element.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'lists.element.get',
            params: {
              IBLOCK_TYPE_ID: 'lists',
              IBLOCK_ID: 47,
              ELEMENT_ID: 6999,
              SELECT: [
                'ID',
                'CODE',
                'NAME',
                'IBLOCK_SECTION_ID',
                'DATE_CREATE',
                'PROPERTY_951',
                'PROPERTY_1003',
              ],
              FILTER: {
                NAME: '%Test%',
                '>=DATE_CREATE': '2025-01-01',
                '<=DATE_CREATE': '2025-12-31',
              },
              ELEMENT_ORDER: {
                NAME: 'asc',
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
          console.info('Fetched elements:', result.length, result[0]?.ID, result[0]?.NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getListElements)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    select = [
        "ID",
        "CODE",
        "NAME",
        "IBLOCK_SECTION_ID",
        "DATE_CREATE",
        "PROPERTY_951",
        "PROPERTY_1003",
    ]

    filter = {
        "NAME": "%Test%",
        "<=DATE_CREATE": "2025-12-31",
        ">=DATE_CREATE": "2025-01-01",
    }

    element_order = {
        "NAME": "asc",
    }

    try:
        bitrix_response = client.lists.element.get(
            iblock_type_id='lists',
            iblock_id=47,
            element_id=6999,
            select=select,
            filter=filter,
            element_order=element_order,
            start=0,
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
- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.element.get',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 47,
                    'ELEMENT_ID' => 6999,
                    'SELECT' => [
                        'ID',
                        'CODE',
                        'NAME',
                        'IBLOCK_SECTION_ID',
                        'DATE_CREATE',
                        'PROPERTY_951',
                        'PROPERTY_1003'
                    ],
                    'FILTER' => [
                        'NAME' => '%Test%',
                        '>=DATE_CREATE' => '2025-01-01',
                        '<=DATE_CREATE' => '2025-12-31'
                    ],
                    'ELEMENT_ORDER' => [
                        'NAME' => 'asc'
                    ],
                    'start' => 0
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching list element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "lists.element.get",
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 47,
            ELEMENT_ID: 6999,
            SELECT: [
                'ID',
                'CODE',
                'NAME',
                `IBLOCK_SECTION_ID`,
                'DATE_CREATE',
                'PROPERTY_951',
                'PROPERTY_1003'
            ],
            FILTER: {
                NAME: '%Test%',
                '>=DATE_CREATE': '2025-01-01',
                '<=DATE_CREATE': '2025-12-31'
            },
            ELEMENT_ORDER: {
                NAME: 'asc'
            }
        },
        function(res) {
            if (res.error()) {
                console.error(res.error());
            } else {
                console.log(res.data());

                if (res.more()) {
                    res.next();
                }
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.get',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 47,
            'ELEMENT_ID' => 6999,
            'SELECT' => [
                'ID',
                'CODE',
                'NAME',
                'IBLOCK_SECTION_ID',
                'DATE_CREATE',
                'PROPERTY_951',
                'PROPERTY_1003'
            ],
            'FILTER' => [
                'NAME' => '%Test%',
                '>=DATE_CREATE' => '2025-01-01',
                '<=DATE_CREATE' => '2025-12-31'
            ],
            'ELEMENT_ORDER' => [
                'NAME' => 'asc'
            ],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "lists.element.get", b24.Params{
    	"IBLOCK_TYPE_ID": "lists",
    	"IBLOCK_ID":      47,
    	"ELEMENT_ID":     6999,
    	"SELECT":         []string{"ID", "CODE", "NAME", "IBLOCK_SECTION_ID", "DATE_CREATE", "PROPERTY_951", "PROPERTY_1003"},
    	"FILTER": b24.Params{
    		"NAME":          "%Test%",
    		"<=DATE_CREATE": "2025-12-31",
    		">=DATE_CREATE": "2025-01-01",
    	},
    	"ELEMENT_ORDER": b24.Params{
    		"NAME": "asc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("lists.element.get: %w", err)
    }

    var items []struct {
    	ID        b24.ID `json:"ID"`
    	Name      string `json:"NAME"`
    	CreatedBy string `json:"CREATED_BY"`
    	Code      string `json:"CODE"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
        "ID": "6999",
        "NAME": "Test Element",
        "IBLOCK_SECTION_ID": null,
        "CREATED_BY": "1269",
        "CODE": "test_element",
        "PROPERTY_951": {
            "3743": "1269",
            "3745": "1271"
        },
        "PROPERTY_1003": {
            "3747": "12/31/2024 11:59:59 pm"
        }
        }
    ],
    "total": 1,
    "time": {
        "start": 1763656328,
        "finish": 1763656328.442951,
        "duration": 0.442950963973999,
        "processing": 0,
        "date_start": "2025-11-19T14:32:08+01:00",
        "date_finish": "2025-11-19T14:32:08+01:00",
        "operating_reset_at": 1763656928,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of elements. If `ELEMENT_ID` or `ELEMENT_CODE` is passed, the array contains one element [(detailed description)](#element).

An empty array means that there are no elements in the list, the elements do not match the filter, or there is no element with the specified `ELEMENT_ID`. In this case, the method does not return an error ||
|| **total**
[`integer`](../../data-types.md) | Total number of elements found. If there are no elements, the field is not returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Element result {#element}

The set of fields depends on `SELECT` and on the fields added to the list. If `SELECT` is not passed, the response contains the `ID`, `IBLOCK_ID`, `NAME`, `IBLOCK_SECTION_ID`, `CREATED_BY`, `BP_PUBLISHED`, and `CODE` fields, the other system fields of the list, and all properties that have values. Field values are returned as strings.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Element identifier ||
|| **IBLOCK_ID**
[`string`](../../data-types.md) | List identifier ||
|| **NAME**
[`string`](../../data-types.md) | Element name ||
|| **IBLOCK_SECTION_ID**
[`string`](../../data-types.md) | Identifier of the list section. `null` if the element is not in a section ||
|| **CREATED_BY**
[`string`](../../data-types.md) | Identifier of the user who created the element ||
|| **CREATED_USER_NAME**
[`string`](../../data-types.md) | Name of the user who created the element. Returned if the list has the "Created by" field ||
|| **MODIFIED_BY**
[`string`](../../data-types.md) | Identifier of the user who modified the element. Returned if the list has the "Modified by" field ||
|| **USER_NAME**
[`string`](../../data-types.md) | Name of the user who modified the element. Returned together with `MODIFIED_BY` ||
|| **BP_PUBLISHED**
[`string`](../../data-types.md) | Indicates whether the element is published within a workflow:
- `Y` — published
- `N` — not published ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the element. `null` if the code is not set ||
|| **DATE_CREATE**
[`string`](../../data-types.md) | Element creation date in the Bitrix24 format, for example `12/09/2025 12:49:30 pm`. Returned if the list has the "Create date" field ||
|| **PROPERTY_PropertyId**
[`object`](../../data-types.md) | Values of the custom property with the `PropertyId` identifier [(detailed description)](#property) ||
|#

The `ACTIVE_FROM`, `ACTIVE_TO`, `SORT`, `PREVIEW_TEXT`, `DETAIL_TEXT`, `PREVIEW_TEXT_TYPE`, and `DETAIL_TEXT_TYPE` fields are returned if they are added to the list.

#### Property Values PROPERTY_PropertyId {#property}

Property values are returned as an object: the key is the identifier of the property value, and the value is the value itself. A single-value property has one key in the object, and a multiple-value property has a key for each value. You need the value identifiers to update or delete a specific value with the [lists.element.update](./lists-element-update.md) method.

```json
{
    "PROPERTY_951": {
        "3743": "1269",
        "3745": "1271"
    }
}
```

In the example, property `951` of the "Bind to employee" type stores two values: employees `1269` and `1271`. The values have the identifiers `3743` and `3745`.

The value format depends on the property type:
- `S`, `N`, `L`, `E`, `S:employee`, `S:ECrm` — a string with the value or the identifier of the list option, element, employee, or CRM object
- `S:Date`, `S:DateTime` — a string with a date in the Bitrix24 format
- `F` — a string with the file identifier. The [lists.element.get.file.url](./lists-element-get-file-url.md) method returns a link to the file
- `S:DiskFile` — an array of identifiers of attached Drive files

Properties without values are not included in the response. The [lists.field.get](../fields/lists-field-get.md) method returns the types of the list properties.

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_REQUIRED_PARAMETERS_MISSING",
    "error_description":"Required parameter is missing"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter is missing ||
|| `400` | `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `400` | `ACCESS_DENIED` | Access denied | Insufficient permissions to read the element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./lists-element-add.md)
- [{#T}](./lists-element-update.md)
- [{#T}](./lists-element-delete.md)
- [{#T}](./lists-element-get-file-url.md)
