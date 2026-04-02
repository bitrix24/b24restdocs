# Delete Region documentgenerator.region.delete

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.region.delete` removes a custom region by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the custom region.

You can obtain the identifier after [creating a region](./document-generator-region-add.md) or by using the [method to get the list of regions](./document-generator-region-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.region.delete
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.region.delete
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.region.delete',
          {
              id: 1
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
          'documentgenerator.region.delete',
          [
              'id' => 1,
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
      'documentgenerator.region.delete',
      {
          id: 1
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
      'documentgenerator.region.delete',
      [
          'id' => 1,
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
        "start": 1742906400,
        "finish": 1742906400.201234,
        "duration": 0.2012341022491455,
        "processing": 0,
        "date_start": "2025-03-25T10:00:00+02:00",
        "date_finish": "2025-03-25T10:00:00+02:00",
        "operating_reset_at": 1742907000,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | The method deletes the region and returns `null`.

It may also return `null` without errors if a non-existent region is specified in the `id` parameter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BX_ERROR",
    "error_description": "There are templates linked to this region"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `BX_ERROR` | There are templates linked to this region | Deletion is not possible while active templates are linked to the region ||
|| `400` | `100` | Could not find value for parameter {id} | Required parameter `id` is missing ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-region-add.md)
- [{#T}](./document-generator-region-update.md)
- [{#T}](./document-generator-region-get.md)
- [{#T}](./document-generator-region-list.md)