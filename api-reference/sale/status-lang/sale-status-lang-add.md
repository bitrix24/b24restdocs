# Add Localization sale.statusLang.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds localization for the order or delivery status.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding status localization in the form of a structure:

```js
fields: {
    statusId: 'value',
    lid: 'value',
    name: 'value',
    description: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **statusId***
[`sale_status.id`](../data-types.md) | Symbolic identifier of the status ||
|| **lid***
[`sale_lang.lid`](../data-types.md) | Language identifier. Current possible values are listed below:

- ar — Arabic
- br — Portuguese
- de — German
- en — English
- fr — French
- hi — Hindi
- id — Indonesian
- in — English (India)
- it — Italian
- ja — Japanese
- la — Spanish
- ms — Malay
- pl — Polish
- de — German
- sc — Chinese (Simplified)
- tc — Chinese (Traditional)
- th — Thai
- tr — Turkish
- ua — Ukrainian
- vn — Vietnamese

The list of available languages can be obtained using the method [sale.statuslang.getlistlangs](./sale-status-lang-get-list-langs.md)
||
|| **name***
[`string`](../../data-types.md) | Name of the status for the created localization ||
|| **description**
[`string`](../../data-types.md) | Description of the status for the created localization ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"statusId":"RD","lid":"de","name":"Returned by Customer","description":"The customer returned the product due to a defect"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.statuslang.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"statusId":"RD","lid":"de","name":"Returned by Customer","description":"The customer returned the product due to a defect"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.statuslang.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sale.statuslang.add',
            {
                fields: {
                    statusId: 'RD',
                    lid: 'de',
                    name: 'Returned by Customer',
                    description: 'The customer returned the product due to a defect'
                }
            }
        );
        
        const result = response.getData().result;
        console.info(result);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.statuslang.add',
                [
                    'fields' => [
                        'statusId'    => 'RD',
                        'lid'         => 'de',
                        'name'        => 'Returned by Customer',
                        'description' => 'The customer returned the product due to a defect'
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding status language: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.statuslang.add',
        {
            fields: {
                statusId: 'RD',
                lid: 'de',
                name: 'Returned by Customer',
                description: 'The customer returned the product due to a defect'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.statuslang.add',
        [
            'fields' =>
            [
                'statusId' => 'RD',
                'lid' => 'de',
                'name' => 'Returned by Customer',
                'description' => 'The customer returned the product due to a defect'
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
    "result": {
        "statusLang": {
            "description": "The customer returned the product due to a defect",
            "lid": "de",
            "name": "Returned by Customer",
            "statusId": "RD"
        }
    },
    "time": {
        "start": 1712218058.514339,
        "finish": 1712218060.34603,
        "duration": 1.831691026687622,
        "processing": 0.042268991470336914,
        "date_start": "2024-04-04T11:07:38+02:00",
        "date_finish": "2024-04-04T11:07:40+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **statusLang**
[`sale_status_lang`](../data-types.md) | Object containing information about the added status localization ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201750000003,
    "error_description": "Duplicate entry for key [statusId, lid]"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201750000003` | Localization for the provided status already exists ||
|| `201750000004` | An unknown localization language identifier `lid` was provided ||
|| `200040300020` | Insufficient permissions to add status localization ||
|| `100` | The `fields` parameter is missing or empty ||
|| `0` | Required fields in the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-status-lang-get-list-langs.md)
- [{#T}](./sale-status-lang-list.md)
- [{#T}](./sale-status-lang-delete-by-filter.md)
- [{#T}](./sale-status-lang-get-fields.md)