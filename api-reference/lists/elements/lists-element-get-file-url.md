# Get File Path lists.element.get.file.url

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required list

The method `lists.element.get.file.url` returns the file path.

## Method Parameters

{% include [Note about required parameters](../../../_includes/required.md) %}

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
|| **ELEMENT_ID***
[`integer`](../../data-types.md) | Identifier of the element.

The identifier can be obtained using the [lists.element.get](./lists-element-get.md) method ||
|| **ELEMENT_CODE***
[`string`](../../data-types.md) | Symbolic code of the element.

The code can be obtained using the [lists.element.get](./lists-element-get.md) method

{% note info "" %}

At least one of the parameters must be specified: `ELEMENT_ID` or `ELEMENT_CODE`

{% endnote %} ||
|| **FIELD_ID***
[`integer`](../../data-types.md) | Identifier of the File or File (Drive) property, without the `PROPERTY_` prefix ||
|#

## Code Examples

{% include [Note about examples](../../../_includes/examples.md) %}



{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":37,"ELEMENT_ID":231,"FIELD_ID":423}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.get.file.url
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":37,"ELEMENT_ID":231,"FIELD_ID":423,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.get.file.url
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.element.get.file.url',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 37,
                ELEMENT_ID: 231,
                FIELD_ID: 423
            }
        );
        
        const result = response.getData().result;
        console.log('File URL:', result);
        
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
                'lists.element.get.file.url',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 37,
                    'ELEMENT_ID' => 231,
                    'FIELD_ID' => 423
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'File URL: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching file URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.element.get.file.url',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 37,
            ELEMENT_ID: 231,
            FIELD_ID: 423  // File (Drive)
        },
        function(res) {
            if (res.error()) {
                console.error(res.error());
            } else {
                const result = res.data();
                console.log('File URL:', result);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.get.file.url',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 37,
            'ELEMENT_ID' => 231,
            'FIELD_ID' => 423
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

### Example Response for File (Drive) Property Type

HTTP Status: **200**

```json
{
    "result": ["/bitrix/tools/disk/uf.php?attachedId=103&action=download&ncc=1"],
    "time": {
        "start": 1763660762,
        "finish": 1763660762.617248,
        "duration": 0.6172480583190918,
        "processing": 0,
        "date_start": "2025-11-19T16:46:02+02:00",
        "date_finish": "2025-11-19T16:46:02+02:00",
        "operating_reset_at": 1763661362,
        "operating": 0
    }
}
```
### Example Response for File Property Type

HTTP Status: **200**

```json
{
    "result": ["/company/lists/37/file/0/6651/PROPERTY_425/32521/?ncc=y&download=y"],
        "time": {
            "start": 1764014727,
            "finish": 1764014727.124893,
            "duration": 0.1248929500579834,
            "processing": 0,
            "date_start": "2025-11-24T17:05:27+02:00",
            "date_finish": "2025-11-24T17:05:27+02:00",
            "operating_reset_at": 1764015327,
            "operating": 0
        }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of links for downloading files.

An empty array means that there are no files in the specified property ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ERROR_ELEMENT_NOT_FOUND` | Element not found | Element with such `ID`/`CODE` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-element-add.md)
- [{#T}](./lists-element-update.md)
- [{#T}](./lists-element-delete.md)
- [{#T}](./lists-element-get.md)