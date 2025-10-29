# Update the section of the universal list lists.section.update

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" or "Edit with restrictions" access permission for the required list

The method `lists.section.update` updates a list section.

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

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|| **SECTION_ID***
[`integer`](../../data-types.md) | Identifier of the section.

The identifier can be obtained using the [lists.section.get](./lists-section-get.md) method ||
|| **SECTION_CODE***
[`string`](../../data-types.md) | Symbolic code of the section.

The code can be obtained using the [lists.section.get](./lists-section-get.md) 

{% note info "" %}

At least one of the parameters must be specified: `SECTION_ID` or `SECTION_CODE` 

{% endnote %} ||
|| **FIELDS***
[`array`](../../data-types.md) | Array of fields.

[Detailed description](#parametr-fields) ||
|# 

### Parameter fields {#parametr-fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../data-types.md) | Name of the section ||
|| **EXTERNAL_ID**
[`string`](../../data-types.md) | External identifier of the section ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier (XML ID) ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **ACTIVE**
[`string`](../../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no ||
|| **PICTURE**
[`array`](../../data-types.md) | Deprecated.

Image. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the image file with extension, `value2` — image in base64 format. 

To delete the image, use the object in the format `{remove: 'Y'}` ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Deprecated.

Description ||
|| **DESCRIPTION_TYPE**
[`string`](../../data-types.md) | Deprecated.

Description type. Possible values:
- `text` — text
- `html` — HTML

Defaults to `text` ||
|| **DETAIL_PICTURE**
[`array`](../../data-types.md) | Deprecated.

Detailed image. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the image file with extension, `value2` — image in base64 format. 

To delete the image, use the object in the format `{remove: 'Y'}` ||
|| **SECTION_PROPERTY**
[`array`](../../data-types.md) | Deprecated.

User properties ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":95,"SECTION_ID":169,"FIELDS":{"NAME":"Updated Marketing Documents","EXTERNAL_ID":"ext_marketing_docs_002","XML_ID":"xml_marketing_docs_002","SORT":600,"ACTIVE":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.section.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":95,"SECTION_ID":169,"FIELDS":{"NAME":"Updated Marketing Documents","EXTERNAL_ID":"ext_marketing_docs_002","XML_ID":"xml_marketing_docs_002","SORT":600,"ACTIVE":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.section.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.section.update',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 95,
                SECTION_ID: 169,
                FIELDS: {
                    NAME: 'Updated Marketing Documents',
                    EXTERNAL_ID: 'ext_marketing_docs_002',
                    XML_ID: 'xml_marketing_docs_002',
                    SORT: 600,
                    ACTIVE: 'Y',
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Updated section with ID:', result);
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
                'lists.section.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 95,
                    'SECTION_ID' => 169,
                    'FIELDS' => [
                        'NAME' => 'Updated Marketing Documents',
                        'EXTERNAL_ID' => 'ext_marketing_docs_002',
                        'XML_ID' => 'xml_marketing_docs_002',
                        'SORT' => 600,
                        'ACTIVE' => 'Y',
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
        echo 'Error updating section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.section.update',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 95,
            SECTION_ID: 169,                       

            FIELDS: {
                NAME: 'Updated Marketing Documents',  
                EXTERNAL_ID: 'ext_marketing_docs_002',
                XML_ID: 'xml_marketing_docs_002',
                SORT: 600,
                ACTIVE: 'Y',
            }
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
        'lists.section.update',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 95,
            'SECTION_ID' => 169,
            'FIELDS' => [
                'NAME' => 'Updated Marketing Documents',
                'EXTERNAL_ID' => 'ext_marketing_docs_002',
                'XML_ID' => 'xml_marketing_docs_002',
                'SORT' => 600,
                'ACTIVE' => 'Y',
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
    "result": true,
    "time": {
        "start": 1761555629,
        "finish": 1761555630.010893,
        "duration": 1.0108931064605713,
        "processing": 1,
        "date_start": "2025-10-27T12:00:29+02:00",
        "date_finish": "2025-10-27T12:00:30+02:00",
        "operating_reset_at": 1761556229,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the section was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| **Code** | **Description** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` |  Required parameter was not provided ||
|| `ACCESS_DENIED` | Insufficient rights to edit the section ||
|| `ERROR_SECTION_NOT_FOUND` |  Section with the specified `SECTION_ID` or `SECTION_CODE` not found ||
|| `ERROR_UPDATE_SECTION` |  Error saving changes to the section ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-section-add.md)
- [{#T}](./lists-section-get.md)
- [{#T}](./lists-section-delete.md)