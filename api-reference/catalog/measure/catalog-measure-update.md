# Update Unit of Measurement catalog.measure.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the unit of measurement.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **Id***
[`catalog_measure.id`](../data-types.md#catalog_measure) | Identifier of the unit of measurement ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the unit of measurement ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`integer`](../../data-types.md) | Unique code of the unit of measurement ||
|| **isDefault**
[`string`](../../data-types.md) | Whether the current unit of measurement is used as the default unit for new products. Possible values:
- `Y` — yes
- `N` — no

Only one unit of measurement from the entire directory can have the value `Y`
||
|| **measureTitle**
[`string`](../../data-types.md) | Title of the unit of measurement
||
|| **symbol**
[`string`](../../data-types.md) | Conditional designation 
||
|| **symbolIntl**
[`string`](../../data-types.md) | International conditional designation
||
|| **symbolLetterIntl**
[`string`](../../data-types.md) | International code letter designation
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8,"fields":{"symbol":"pair","symbolLetterIntl":"nrp","symbolIntl":"pr. 2"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.measure.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8,"fields":{"symbol":"pair","symbolLetterIntl":"nrp","symbolIntl":"pr. 2"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.measure.update
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.measure.update', 
        {
            id: 8,
            fields: {
                symbol: 'pair',
                symbolLetterIntl: 'nrp',
                symbolIntl: 'pr. 2'
        }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.measure.update',
        [
            'id' => 8,
            'fields' => [
                'symbol' => 'pair',
                'symbolLetterIntl' => 'nrp',
                'symbolIntl' => 'pr. 2'
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
        "measure": {
            "code": 715,
            "id": 8,
            "isDefault": "N",
            "measureTitle": "Pair",
            "symbol": "pair",
            "symbolIntl": "pr. 2",
            "symbolLetterIntl": "nrp"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-04-05T16:24:46+02:00",
        "date_finish": "2024-04-05T16:24:46+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **measure**
[`catalog_measure`](../data-types.md#catalog_measure) | Object with information about the updated unit of measurement ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: code"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | No access to edit
||
|| `200600000000` | A unit of measurement with the specified `code` already exists
||
|| `200600000010` | A unit of measurement with the `isDefault` parameter set to `Y` already exists
||
|| `200600000020` | A unit of measurement with the specified identifier does not exist
||
|| `100` | The `id` parameter is not specified
||
|| `100` | The `fields` parameter is not specified or is empty
||
|| `0` | Required fields in the `fields` structure are not provided
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-measure-add.md)
- [{#T}](./catalog-measure-get.md)
- [{#T}](./catalog-measure-list.md)
- [{#T}](./catalog-measure-delete.md)
- [{#T}](./catalog-measure-get-fields.md)