# Update Dataset biconnector.dataset.update

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Analyst Workspace" section

The method `biconnector.dataset.update` updates an existing dataset.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the dataset, can be obtained using the methods [biconnector.dataset.list](./biconnector-dataset-list.md) and [biconnector.dataset.add](./biconnector-dataset-add.md) ||
|| **fields***
[`object`](../../data-types.md) | An object containing the updated data.
Object format: 

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

[Detailed description below](#fields)||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **description**
[`string`](../../data-types.md) | Description of the dataset ||
|#

To change the fields of the dataset, use the method [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md).

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX.rest.callMethod(
        'biconnector.dataset.update',
        {
            id: 10,
            fields: {
                "description": "New description",
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
        "id": 10,
        "fields": {
            "description": "New description"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 10,
        "fields": {
            "description": "New description"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.update
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.update',
        [
            'id' => 10,
            'fields' => [
                'description' => 'New description'
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
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **200**

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
|| `VALIDATION_ID_NOT_PROVIDED` | `ID is missing.` | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | `ID has to be a positive integer.` | Invalid ID format ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | `Fields not provided.` | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | `Unknown parameters: #LIST_OF_PARAMS#` | Unknown parameters detected: #LIST\_OF\_PARAMS#. ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | `Field "#TITLE#" is required.` | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | `Field "#TITLE#" is read only.` | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | `Field "#TITLE#" is immutable.` | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | `Field "#TITLE#" must be of type #TYPE#.` | Field #TITLE# must be of type #TYPE# ||
|| `DATASET_NOT_FOUND` | `Dataset was not found.` | Dataset not found ||
|| `INVALID_METHOD` | `Use the method "biconnector.dataset.fields.update" to update the dataset fields.` | To update fields, use the method `biconnector.dataset.fields.update` ||
|| `-` | `Error updating dataset.` | Error updating dataset ||
|| `VALIDATION_DUPLICATE_FIELD_CODE` | `Duplicate values found in the "code" parameter: #LIST_CODES#` | Duplicates found in the `externalCode` parameter of dataset fields ||
|| `VALIDATION_DUPLICATE_FIELD_NAME` | `Duplicate values found in the "name" parameter: #LIST_NAMES#` | Duplicates found in the `name` parameter of dataset fields ||
|| `VALIDATION_FIELD_MISSING_REQUIRED_PARAMETERS` | `Field must include the required parameters: "name", "externalCode" and "type".` | Field must include the parameters `name`, `externalCode`, and `type` ||
|| `VALIDATION_FIELD_NAME_INVALID_FORMAT` | `Field "name" has to start with an uppercase Latin character. Possible entry includes uppercase Latin characters (A-Z), numbers (0-9) and underscores.` | Invalid field name format. Name must start with a letter, only uppercase Latin letters `(A-Z)`, numbers, and `_` are allowed ||
|| `VALIDATION_FIELD_NAME_TOO_LONG` | `Field "name" must not exceed 32 characters.` | Field name must not exceed 32 characters ||
|| `VALIDATION_FIELD_INVALID_TYPE` | `Invalid field type.` | Invalid field type ||
|| `-` | `Error adding dataset` | Error adding dataset ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-fields.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)