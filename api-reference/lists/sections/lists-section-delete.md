# Delete Section of Universal List lists.section.delete

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Edit" access permission for the required list

The method `lists.section.delete` removes a section from the list.

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
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":95,"SECTION_ID":169}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.section.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":95,"SECTION_ID":169,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.section.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.section.delete',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 95,
                SECTION_ID: 169,
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted section:', result);
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
                'lists.section.delete',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 95,
                    'SECTION_ID' => 169,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.section.delete',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 95,
            SECTION_ID: 169,
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
        'lists.section.delete',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 95,
            'SECTION_ID' => 169,
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
        "start": 1761557326,
        "finish": 1761557326.424922,
        "duration": 0.42492198944091797,
        "processing": 0,
        "date_start": "2025-10-27T12:28:46+02:00",
        "date_finish": "2025-10-27T12:28:46+02:00",
        "operating_reset_at": 1761557926,
        "operating": 0.13587212562561035
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the section was successfully deleted ||
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
|| `ACCESS_DENIED` | Insufficient permissions to delete the section ||
|| `ERROR_SECTION_NOT_FOUND` |  Section with the specified `SECTION_ID` or `SECTION_CODE` not found ||
|| `ERROR_DELETE_SECTION` |  Error while deleting the section ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-section-add.md)
- [{#T}](./lists-section-update.md)
- [{#T}](./lists-section-get.md)