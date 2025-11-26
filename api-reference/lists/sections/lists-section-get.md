# Get section parameters or list of sections using lists.section.get

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for the required list

The method `lists.section.get` returns a section or a list of sections.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

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

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method. ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method.

{% note info "" %}

At least one of the parameters: `IBLOCK_ID` or `IBLOCK_CODE` must be specified.

{% endnote %} ||
|| **FILTER**
[`object`](../../data-types.md) | Object for filtering sections `{"field_1": "value_1", ... "field_N": "value_N"}`. The filterable field can take the following values:

- `ID` — section identifier  
- `CODE` — symbolic code of the section  
- `XML_ID` — external identifier (XML ID)
- `EXTERNAL_ID` — external section identifier
- `NAME` — section name
- `ACTIVE` — activity status
- `GLOBAL_ACTIVE` — global activity
- `IBLOCK_ACTIVE` — information block activity status
- `IBLOCK_NAME` — information block name
- `IBLOCK_TYPE` — information block type identifier
- `IBLOCK_XML_ID` — external identifier of the information block (XML ID)
- `IBLOCK_EXTERNAL_ID` — external identifier of the information block
- `DEPTH_LEVEL` — nesting level
- `LEFT_MARGIN` — left boundary of the tree
- `RIGHT_MARGIN` — right boundary of the tree
- `LEFT BORDER` — left boundary
- `RIGHT BORDER` — right boundary
- `TIMESTAMP_X` — time of the last modification
- `DATE_CREATE` — section creation date  
- `CREATED_BY` — identifier of the user who created the section  
- `MODIFIED_BY` — identifier of the user who modified the section  

You can specify the type of filtering before the filterable field name:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to
- `%` — substring search

By default, records are not filtered. ||
|| **SELECT**
[`array`](../../data-types.md) | Array with fields to select. Available fields:
- `ID` — section identifier
- `CODE` — symbolic code of the section
- `XML_ID` — external identifier (XML ID)
- `EXTERNAL_ID` — external section identifier
- `IBLOCK_SECTION_ID` — identifier of the parent section
- `TIMESTAMP_X` — time of the last modification
- `SORT` — sorting
- `NAME` — section name
- `ACTIVE` — activity status
- `GLOBAL_ACTIVE` — global activity
- `PICTURE` — picture (deprecated)
- `DESCRIPTION` — description (deprecated)
- `DESCRIPTION_TYPE` — description type (deprecated)
- `LEFT_MARGIN` — left boundary of the tree
- `RIGHT_MARGIN` — right boundary of the tree
- `DEPTH_LEVEL` — nesting level
- `SEARCHABLE_CONTENT` — searchable content
- `SECTION_PAGE_URL` — page URL (deprecated)
- `MODIFIED_BY` — identifier of the user who modified the section
- `DATE_CREATE` — section creation date
- `CREATED_BY` — identifier of the user who created the section
- `DETAIL_PICTURE` — detailed picture (deprecated)
  
If no fields are specified, all available fields are returned by default. ||
|#

{% note info "" %}

To retrieve data for a single section, specify its identifier in FILTER. Without a filter, the method will return a list of all sections.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":95,"FILTER":{"ID":169,"ACTIVE":"Y","NAME":"%marketing%","<=DATE_CREATE":"2025-12-31",">=DATE_CREATE":"2025-01-01"},"SELECT":["ID","CODE","XML_ID","EXTERNAL_ID","IBLOCK_SECTION_ID","TIMESTAMP_X","SORT","NAME","ACTIVE","GLOBAL_ACTIVE","LEFT_MARGIN","RIGHT_MARGIN","DEPTH_LEVEL","SEARCHABLE_CONTENT","MODIFIED_BY","DATE_CREATE","CREATED_BY"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.section.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":95,"FILTER":{"ID":169,"ACTIVE":"Y","NAME":"%marketing%","<=DATE_CREATE":"2025-12-31",">=DATE_CREATE":"2025-01-01"},"SELECT":["ID","CODE","XML_ID","EXTERNAL_ID","IBLOCK_SECTION_ID","TIMESTAMP_X","SORT","NAME","ACTIVE","GLOBAL_ACTIVE","LEFT_MARGIN","RIGHT_MARGIN","DEPTH_LEVEL","SEARCHABLE_CONTENT","MODIFIED_BY","DATE_CREATE","CREATED_BY"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.section.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.section.get',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 95,
                FILTER: {
                    ID: 169,
                    ACTIVE: 'Y',
                    NAME: '%marketing%',
                    '>=DATE_CREATE': '2025-01-01',
                    '<=DATE_CREATE': '2025-12-31'
                },
                SELECT: [
                    'ID',
                    'CODE',
                    'XML_ID',
                    'EXTERNAL_ID',
                    'IBLOCK_SECTION_ID',
                    'TIMESTAMP_X',
                    'SORT',
                    'NAME',
                    'ACTIVE',
                    'GLOBAL_ACTIVE',
                    'LEFT_MARGIN',
                    'RIGHT_MARGIN',
                    'DEPTH_LEVEL',
                    'SEARCHABLE_CONTENT',
                    'MODIFIED_BY',
                    'DATE_CREATE',
                    'CREATED_BY',
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved sections:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.section.get',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 95,
                    'FILTER' => [
                        'ID' => 169,
                        'ACTIVE' => 'Y',
                        'NAME' => '%marketing%',
                        '>=DATE_CREATE' => '2025-01-01',
                        '<=DATE_CREATE' => '2025-12-31'
                    ],
                    'SELECT' => [
                        'ID',
                        'CODE',
                        'XML_ID',
                        'EXTERNAL_ID',
                        'IBLOCK_SECTION_ID',
                        'TIMESTAMP_X',
                        'SORT',
                        'NAME',
                        'ACTIVE',
                        'GLOBAL_ACTIVE',
                        'LEFT_MARGIN',
                        'RIGHT_MARGIN',
                        'DEPTH_LEVEL',
                        'SEARCHABLE_CONTENT',
                        'MODIFIED_BY',
                        'DATE_CREATE',
                        'CREATED_BY',
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'lists.section.get',
    {
        IBLOCK_TYPE_ID: 'lists',
        IBLOCK_ID: 95,
        
        FILTER: {
            'ID': 169,
            'ACTIVE': 'Y',
            'NAME': '%marketing%',
            '>=DATE_CREATE': '2025-01-01',
            '<=DATE_CREATE': '2025-12-31'
        },

        SELECT: [
            'ID',
            'CODE',
            'XML_ID',
            'EXTERNAL_ID',
            'IBLOCK_SECTION_ID',
            'TIMESTAMP_X',
            'SORT',
            'NAME',
            'ACTIVE',
            'GLOBAL_ACTIVE',
            'LEFT_MARGIN',
            'RIGHT_MARGIN',
            'DEPTH_LEVEL',
            'SEARCHABLE_CONTENT',
            'MODIFIED_BY',
            'DATE_CREATE',
            'CREATED_BY',
        ]
    },
    function(result) {
        if (result.error()) {
            console.error(result.error());
        } else {
            console.log(result.data());
        }
    }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.section.get',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 95,
            'FILTER' => [
                'ID' => 169,
                'ACTIVE' => 'Y',
                'NAME' => '%marketing%',
                '>=DATE_CREATE' => '2025-01-01',
                '<=DATE_CREATE' => '2025-12-31'
            ],
            'SELECT' => [
                'ID',
                'CODE',
                'XML_ID',
                'EXTERNAL_ID',
                'IBLOCK_SECTION_ID',
                'TIMESTAMP_X',
                'SORT',
                'NAME',
                'ACTIVE',
                'GLOBAL_ACTIVE',
                'LEFT_MARGIN',
                'RIGHT_MARGIN',
                'DEPTH_LEVEL',
                'SEARCHABLE_CONTENT',
                'MODIFIED_BY',
                'DATE_CREATE',
                'CREATED_BY',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "169",
            "CODE": "marketing_documents",
            "XML_ID": "ext_marketing_docs_002",
            "EXTERNAL_ID": "ext_marketing_docs_002",
            "IBLOCK_SECTION_ID": null,
            "TIMESTAMP_X": "10/27/2025 12:00:30 pm",
            "SORT": "600",
            "NAME": "Updated Marketing Documents",
            "ACTIVE": "Y",
            "GLOBAL_ACTIVE": "Y",
            "LEFT_MARGIN": "17",
            "RIGHT_MARGIN": "18",
            "DEPTH_LEVEL": "1",
            "SEARCHABLE_CONTENT": "UPDATED MARKETING DOCUMENTS\r\n",
            "MODIFIED_BY": "1269",
            "DATE_CREATE": "10/27/2025 11:36:56 am",
            "CREATED_BY": "1269"
        }
    ],
    "total": 1,
    "time": {
        "start": 1761556287,
        "finish": 1761556287.844517,
        "duration": 0.8445169925689697,
        "processing": 0,
        "date_start": "2025-10-27T12:11:27+02:00",
        "date_finish": "2025-10-27T12:11:27+02:00",
        "operating_reset_at": 1761556887,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Data of the section or an array of sections. The result depends on the SELECT parameter.

An empty array means that there are no sections in the list, or the sections do not match the filter. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP status: **400**

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
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter is not provided. ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the section. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-section-add.md)
- [{#T}](./lists-section-update.md)
- [{#T}](./lists-section-delete.md)