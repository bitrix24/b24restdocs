# Delete universal list lists.delete

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required list

The method `lists.delete` removes a universal list.

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type

The identifier can be obtained using the method [lists.get-iblock-type-id](./lists-get-iblock-type-id.md) ||
|| **IBLOCK_ID***
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the method [lists.get](../lists/lists-get.md) ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the method [lists.get](../lists/lists-get.md)

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":109}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":109,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.delete
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod(
          'lists.delete',
          {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 109,
          }
      );
    
      const result = response.getData().result;
      console.log('Deleted list with ID:', result);
    
      processResult(result);
  } catch( error ) {
      console.error('Error:', error);
  }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.delete',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 109
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'lists.delete',
      {
         IBLOCK_TYPE_ID: 'lists',
         IBLOCK_ID: 109
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
        'lists.delete',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 109
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1764698473,
        "finish": 1764698473.663904,
        "duration": 0.6639039516448975,
        "processing": 0,
        "date_start": "2025-12-02T17:01:13+01:00",
        "date_finish": "2025-12-02T17:01:13+01:00",
        "operating_reset_at": 1764699073,
        "operating": 0.16965889930725098
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the list was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error":"ERROR_IBLOCK_NOT_FOUND",
    "error_description":"Iblock not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | List with such `IBLOCK_ID` or `IBLOCK_CODE` not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to delete the list ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue exploring 

- [{#T}](./lists-add.md)
- [{#T}](./lists-update.md)
- [{#T}](./lists-get.md)
- [{#T}](./lists-get-iblock-type-id.md)