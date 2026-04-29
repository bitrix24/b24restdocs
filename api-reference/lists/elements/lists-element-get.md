# Get Element Parameters or List of Elements lists.element.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required list

The method `lists.element.get` returns an element or a list of elements.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type ||
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
- `=` — exact match
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
- `=` — equal, exact match, used by default
- `!=` — not equal
- `!` — not equal

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
- `S:map_yandex` — binding to Yandex.Map
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

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'lists.element.get',
            {
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
                    'PROPERTY_1003'
                ],
                FILTER: {
                    NAME: '%Test%',
                    '>=DATE_CREATE': '2025-01-01',
                    '<=DATE_CREATE': '2025-12-31'
                },
                ELEMENT_ORDER: {
                    NAME: 'asc'
                },
                start: 0
            }
        );

        const result = response.getData().result;
        console.log('Fetched elements:', result);
        processResult(result);
    } catch (error) {
        console.error('Error:', error);
    }
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
[`array`](../../data-types.md) | Data of the element or an array of elements. The result depends on the SELECT parameter.

An empty array means that there are no elements in the list, or the elements do not match the filter ||
|| **total**
[`integer`](../../data-types.md) | Total number of elements ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

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
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter is missing ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to read the element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-element-add.md)
- [{#T}](./lists-element-update.md)
- [{#T}](./lists-element-delete.md)
- [{#T}](./lists-element-get-file-url.md)