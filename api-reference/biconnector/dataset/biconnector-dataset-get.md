# Get dataset by id biconnector.dataset.get

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Analyst's workspace" section

The method `biconnector.dataset.get` returns information about a dataset by its identifier.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the dataset, which can be obtained using the methods [biconnector.dataset.list](./biconnector-dataset-list.md) and [biconnector.dataset.add](./biconnector-dataset-add.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'biconnector.dataset.get',
        {
            id: 2,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.get
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.get',
        [
            'id' => 2
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
        "item": {
            "id": 2,
            "type": "rest",
            "name": "rest_dataset11111",
            "description": "new__2_",
            "externalCode": "extrnalCode",
            "externalName": "extranalName",
            "dateCreate": "2025-03-26 15:28:06",
            "dateUpdate": "2025-03-27 07:47:43",
            "createdById": 1,
            "updatedById": 1,
            "externalId": 275,
            "fields": [
                {"id": 224, "datasetId": 2, "type": "int", "name": "ID", "externalCode": "ID", "visible": true},
                {"id": 225, "datasetId": 2, "type": "string", "name": "NAME", "externalCode": "NAME", "visible": true},
                {"id": 226, "datasetId": 2, "type": "string", "name": "SURNAME", "externalCode": "SURNAME", "visible": true},
                {"id": 227, "datasetId": 2, "type": "double", "name": "SCORE", "externalCode": "SCORE", "visible": true},
                {"id": 228, "datasetId": 2, "type": "date", "name": "DATA", "externalCode": "DATA", "visible": true},
                {"id": 229, "datasetId": 2, "type": "datetime", "name": "TIME", "externalCode": "TIME", "visible": true}
            ]
        }
    },
    "time": {
        "start": 1743061675.963969,
        "finish": 1743061676.064591,
        "duration": 0.10062193870544434,
        "processing": 0.011152029037475586,
        "date_start": "2025-03-27T07:47:55+00:00",
        "date_finish": "2025-03-27T07:47:56+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`item`](../../data-types.md) | Root element of the response. Contains information about the dataset fields. Field descriptions can be found in the article [Datasets: Overview of Methods](./index.md#dataset) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#    

## Error Handling

HTTP status: **200**

```json
{
    "error": "VALIDATION_ID_NOT_PROVIDED",
    "error_description": "ID is missing."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `DATASET_NOT_FOUND` | Dataset was not found. | Dataset not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-fields.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)