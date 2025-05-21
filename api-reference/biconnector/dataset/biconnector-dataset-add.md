# Create Dataset biconnector.dataset.add

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Analyst's workspace" section

The method `biconnector.dataset.add` creates a new dataset associated with a data source.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing data for creating a new dataset. The object format is:

```
{
    "field_1": "value_1",
    "field_2": "value_2",
    ...,
    "field_n": "value_n"
}
```

- `field_n` — field name
- `value_n` — field value

[Detailed description below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Dataset name ||
|| **externalName***
[`string`](../../data-types.md) | Dataset name in the external source, in the application ||
|| **externalCode***
[`string`](../../data-types.md) | Unique dataset code in the external source, used when retrieving data ||
|| **sourceId***
[`integer`](../../data-types.md) | Source identifier, can be obtained using the methods [biconnector.source.list](../source/biconnector-source-list.md) or [biconnector.source.add](../source/biconnector-source-add.md) ||
|| **description**
[`string`](../../data-types.md) | Dataset description ||
|| **fields***
[`array`](../../data-types.md) | List of dataset fields, [(Detailed description)](./index.md#fields) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'biconnector.dataset.add',
        {
            fields: {
                "sourceId": 3,
                "name": "rest_dataset",
                "externalName": "externalName",
                "externalCode": "externalCode",
                "description": "Dataset description",
                "fields": [
                    { "type": "int", "name": "ID", "externalCode": "ID" },
                    { "type": "string", "name": "NAME", "externalCode": "NAME" },
                    { "type": "string", "name": "SURNAME", "externalCode": "SURNAME" },
                    { "type": "double", "name": "SCORE", "externalCode": "SCORE" },
                    { "type": "date", "name": "DATE", "externalCode": "DATE" },
                    { "type": "datetime", "name": "TIME", "externalCode": "TIME" }
                ]
            }
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
    -d '{
        "fields": {
            "sourceId": 3,
            "name": "rest_dataset",
            "externalName": "externalName",
            "externalCode": "externalCode",
            "description": "Dataset description",
            "fields": [
                { "type": "int", "name": "ID", "externalCode": "ID" },
                { "type": "string", "name": "NAME", "externalCode": "NAME" },
                { "type": "string", "name": "SURNAME", "externalCode": "SURNAME" },
                { "type": "double", "name": "SCORE", "externalCode": "SCORE" },
                { "type": "date", "name": "DATE", "externalCode": "DATE" },
                { "type": "datetime", "name": "TIME", "externalCode": "TIME" }
            ]
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "sourceId": 3,
            "name": "rest_dataset",
            "externalName": "externalName",
            "externalCode": "externalCode",
            "description": "Dataset description",
            "fields": [
                { "type": "int", "name": "ID", "externalCode": "ID" },
                { "type": "string", "name": "NAME", "externalCode": "NAME" },
                { "type": "string", "name": "SURNAME", "externalCode": "SURNAME" },
                { "type": "double", "name": "SCORE", "externalCode": "SCORE" },
                { "type": "date", "name": "DATE", "externalCode": "DATE" },
                { "type": "datetime", "name": "TIME", "externalCode": "TIME" }
            ]
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.add
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.add',
        [
            'fields' => [
                'sourceId' => 3,
                'name' => 'rest_dataset',
                'externalName' => 'externalName',
                'externalCode' => 'externalCode',
                'description' => 'Dataset description',
                'fields' => [
                    [ 'type' => 'int', 'name' => 'ID', 'externalCode' => 'ID' ],
                    [ 'type' => 'string', 'name' => 'NAME', 'externalCode' => 'NAME' ],
                    [ 'type' => 'string', 'name' => 'SURNAME', 'externalCode' => 'SURNAME' ],
                    [ 'type' => 'double', 'name' => 'SCORE', 'externalCode' => 'SCORE' ],
                    [ 'type' => 'date', 'name' => 'DATE', 'externalCode' => 'DATE' ],
                    [ 'type' => 'datetime', 'name' => 'TIME', 'externalCode' => 'TIME' ]
                ]
            ]
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
    "result": {
      "id": 10
    },
    "time": {
        "start": 1725013197.635808,
        "finish": 1725013198.580873,
        "duration": 0.9450650215148926,
        "processing": 0.6822988986968994,
        "date_start": "2024-08-30T12:19:57+02:00",
        "date_finish": "2024-08-30T12:19:58+02:00",
        "operating": 0
    }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the created dataset ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#
## Error Handling

HTTP Status: **200**

```json
{
    "error": "VALIDATION_FIELDS_NOT_PROVIDED",
    "error_description": "Fields not provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

## Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | `Fields not provided.` | Fields were not provided in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | `Unknown parameters: #LIST_OF_PARAMS#` | Unknown parameters detected: list ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | `Field "#TITLE#" is required.` | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | `Field "#TITLE#" is read only.` | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | `Field "#TITLE#" is immutable.` | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | `Field "#TITLE#" must be of type #TYPE#.` | Field #TITLE# must be of type #TYPE# ||
|| `SOURCE_NOT_FOUND` | `Source was not found.` | Source not found ||
|| `DATASET_ALREADY_EXIST` | `Dataset with this name already exists.` | A dataset with this name already exists ||
|| `VALIDATION_DATASET_NAME_INVALID` | `Dataset name has to start with a lowercase Latin character. Possible entry includes lowercase Latin characters (a-z), numbers (0-9) and underscores.` | Invalid dataset name format. The name must start with a letter, only lowercase Latin letters `(a-z)`, numbers, and `_` are allowed ||
|| `VALIDATION_DATASET_NAME_TOO_LONG` | `Dataset name must not exceed 230 characters.` | Dataset name must not exceed 230 characters ||
|| `VALIDATION_DUPLICATE_FIELD_CODE` | `Duplicate values found in the "code" parameter: #LIST_CODES#` | Duplicates found in the `externalCode` parameter of dataset fields ||
|| `VALIDATION_DUPLICATE_FIELD_NAME` | `Duplicate values found in the "name" parameter: #LIST_NAMES#` | Duplicates found in the `name` parameter of dataset fields ||
|| `VALIDATION_FIELD_MISSING_REQUIRED_PARAMETERS` | `Field must include the required parameters: "name", "externalCode" and "type".` | Field must include the parameters `name`, `externalCode`, and `type` ||
|| `VALIDATION_FIELD_NAME_INVALID_FORMAT` | `Field "name" has to start with an uppercase Latin character. Possible entry includes uppercase Latin characters (A-Z), numbers (0-9) and underscores.` | Invalid field name format. The name must start with a letter, only uppercase Latin letters `(A-Z)`, numbers, and `_` are allowed ||
|| `VALIDATION_FIELD_NAME_TOO_LONG` | `Field "name" must not exceed 32 characters.` | Field name must not exceed 32 characters ||
|| `VALIDATION_FIELD_INVALID_TYPE` | `Invalid field type.` | Invalid field type ||
|| `-` | `Error adding dataset` | Error adding dataset ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-fields.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)