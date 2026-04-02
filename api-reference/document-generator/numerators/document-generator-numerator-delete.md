# Delete the documentgenerator.numerator.delete

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.numerator.delete` removes a numerator by its identifier.

{% note warning "" %}

You can only delete a numerator that was created using the `documentgenerator.numerator.add` method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the numerator.

You can obtain the identifier after [creating a numerator](./document-generator-numerator-add.md) or by using the [method to get the list of numerators](./document-generator-numerator-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.numerator.delete
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.numerator.delete
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.numerator.delete',
          {
              id: 57
          }
      );

      const result = response.getData().result;
      console.log(result);
  }
  catch (error)
  {
      console.error(error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service->core->call(
          'documentgenerator.numerator.delete',
          [
              'id' => 57,
          ]
      );

      $result = $response->getResponseData()->getResult();
      var_dump($result);
  } catch (Throwable $e) {
      echo $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'documentgenerator.numerator.delete',
      {
          id: 57
      },
      function(result)
      {
          if (result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.log(result.data());
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'documentgenerator.numerator.delete',
      [
          'id' => 57,
      ]
  );

  print_r($result);
  ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1774362582,
        "finish": 1774362582.552183,
        "duration": 0.5521829128265381,
        "processing": 0,
        "date_start": "2026-03-24T17:29:42+01:00",
        "date_finish": "2026-03-24T17:29:42+01:00",
        "operating_reset_at": 1774363182,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | The method deletes the numerator and returns `null` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DOCGEN_ACCESS_ERROR",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\Main\Numerator\Numerator constructor must be public | Required parameter `id` not provided ||
|| `400` | `100` | Could not construct parameter {numerator} | Non-existent or invalid numerator identifier provided ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|| `400` | `DOCGEN_ACCESS_ERROR` | Access denied | Cannot delete a numerator that was not created by the [documentgenerator.numerator.add](./document-generator-numerator-add.md) method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-numerator-add.md)
- [{#T}](./document-generator-numerator-update.md)
- [{#T}](./document-generator-numerator-get.md)
- [{#T}](./document-generator-numerator-list.md)