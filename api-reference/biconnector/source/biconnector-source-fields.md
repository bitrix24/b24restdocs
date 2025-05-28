# Get Source Fields biconnector.source.fields

> Scope: [`biconnector`](../../scopes/permissions.md)
> 
> Who can execute the method: any user

The method `biconnector.source.fields` returns a description of the source fields. A table with the description of standard fields can be found in the article [Sources: Overview of Methods](./index.md#fields).

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'biconnector.source.fields',
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.source.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.fields
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.source.fields',
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
        "title": "title",
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
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
        "title": "code",
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
        "title": "active",
        "type": "boolean",
        "isRequired": false,
        "isReadOnly": false,
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
        "title": "connectorId",
        "type": "integer",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": true,
        "isMultiple": false
      },
      {
        "title": "settings",
        "type": "array",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": true
      }
    ]
  },
  "time": {
    "start": 1742896156.448294,
    "finish": 1742896156.503291,
    "duration": 0.05499696731567383,
    "processing": 0.0004570484161376953,
    "date_start": "2025-03-25T09:49:16+00:00",
    "date_finish": "2025-03-25T09:49:16+00:00"
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
- `field_n` — source field
- `value_n` — [field information](../connector/index.md#description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-source-update.md)
- [{#T}](./biconnector-source-get.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-add.md)