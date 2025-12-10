# Get the information block type ID lists.get.iblock.type.id

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for lists

The method `lists.get.iblock.type.id` returns the information block type ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_ID*** 
[`integer`](../../data-types.md) | Information block ID.

The ID can be obtained using the [lists.get](../lists/lists-get.md) method ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

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
    -d '{"IBLOCK_ID":87}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.get.iblock.type.id
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_ID":87,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.get.iblock.type.id
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.get.iblock.type.id',
            {
                IBLOCK_ID: 87,
            }
        );
        
        const result = response.getData().result;
        console.log('Data:', result);
        
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
                'lists.get.iblock.type.id',
                [
                    'IBLOCK_ID' => 87
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
     'lists.get.iblock.type.id',
     {
        IBLOCK_ID: 87
     },
        function(res) {
            if (res.error()) {
                console.error(res.error());
            } else {
                console.log(res.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.get.iblock.type.id',
        [
            'IBLOCK_ID' => 87
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
    "result": "lists",
    "time": {
        "start": 1764775444,
        "finish": 1764775444.66342,
        "duration": 0.6634199619293213,
        "processing": 0,
        "date_start": "2025-12-03T18:24:04+01:00",
        "date_finish": "2025-12-03T18:24:04+01:00",
        "operating_reset_at": 1764776044,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../data-types.md) | Information block type ID.

Returns `null` if the information block is not found or the parameters are incorrect ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-add.md)
- [{#T}](./lists-update.md)
- [{#T}](./lists-get.md)
- [{#T}](./lists-delete.md)