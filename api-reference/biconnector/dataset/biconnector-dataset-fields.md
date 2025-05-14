# Get Fields of the Dataset biconnector.dataset.fields

> Scope: [`biconnector`](../../scopes/permissions.md)
> 
> Who can execute the method: a user with access to the "Analyst Workspace" section

The method `biconnector.dataset.fields` returns the description of the dataset fields. A table with the description of standard fields can be found in the article [Datasets: Overview of Methods](./index.md#dataset).

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'biconnector.dataset.fields',
        {},
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        },
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.fields
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.fields',
        []
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
    "fields": [
      {
        "title": "id",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "sourceId",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": true,
        "isMultiple": false
      },
      {
        "title": "name",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": true,
        "isMultiple": false
      },
      {
        "title": "type",
        "type": "string",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "description",
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "externalName",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": true,
        "isMultiple": false
      },
      {
        "title": "externalCode",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": true,
        "isMultiple": false
      },
      {
        "title": "externalId",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "dateCreate",
        "type": "datetime",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "dateUpdate",
        "type": "datetime",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "createdById",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "updatedById",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false
      },
      {
        "title": "fields",
        "type": "array",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": true
      }
    ]
  },
  "time": {
    "start": 1740757652.264398,
    "finish": 1740757652.343882,
    "duration": 0.0794839859008789,
    "processing": 2.002716064453125e-5,
    "date_start": "2025-02-28T15:47:32+00:00",
    "date_finish": "2025-02-28T15:47:32+00:00"
  }
}
```

## Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object in the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...
    field_n: value_n,
}
```

where:
- `field_n` — dataset field
- `value_n` — information about the field in the format [biconnector_rest_field_description](../connector/index.md#description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)