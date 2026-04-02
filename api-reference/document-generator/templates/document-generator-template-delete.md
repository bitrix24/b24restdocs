# Delete Template documentgenerator.template.delete

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.delete` removes a template.

1. If there are documents in the system associated with the specified template, the template is marked as deleted to preserve the document associations. You can retrieve a list of templates marked as deleted using the method [documentgenerator.template.list](./document-generator-template-list.md) with a filter for the parameter `isDeleted = "Y"`.
2. If the template has no associated documents, the record for the template is completely removed.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the template.

You can obtain the template identifier after [creating a template](./document-generator-template-add.md) or by using the [get template list](./document-generator-template-list.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":59}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.delete
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":59,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.delete
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.template.delete',
          {
              id: 59
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
          'documentgenerator.template.delete',
          [
              'id' => 59,
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
      'documentgenerator.template.delete',
      {
          id: 59
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
      'documentgenerator.template.delete',
      [
          'id' => 59,
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
        "start": 1773914210,
        "finish": 1773914211.747039,
        "duration": 1.7470390796661377,
        "processing": 1,
        "date_start": "2026-03-19T12:56:50+01:00",
        "date_finish": "2026-03-19T12:56:51+01:00",
        "operating_reset_at": 1773914810,
        "operating": 1.222714900970459
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | The method deletes the template and returns `null` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Template not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Template constructor must be public | Required parameter `id` not provided ||
|| `400` | `0` | Template not found | Template with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-list.md)
- [{#T}](./document-generator-template-get-fields.md)