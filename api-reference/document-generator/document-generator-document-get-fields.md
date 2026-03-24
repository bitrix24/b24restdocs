# Get the list of fields for the document documentgenerator.document.getfields

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify documents

The method `documentgenerator.document.getfields` returns the fields of the document, along with their current and base values.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Document identifier.

You can obtain the document identifier after [creating the document](./document-generator-document-add.md) or by using the [get document list method](./document-generator-document-list.md) ||
|| **values**
[`object`](../data-types.md) | Optional parameter.

Field values that the method temporarily substitutes before forming `documentFields`.

Use `values` to check in advance how the values will be interpreted and displayed when [creating](./document-generator-document-add.md) or [updating](./document-generator-document-update.md) the document.

If `values` is not provided, the method will return a structure of fields with current and base values ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"values":{"DocumentNumber":"DG-2026-001","CurrentDate":"2026-03-18T00:00:00+02:00","ClientName":"Ltd. Chamomile","ClientPhone":"+1 999 123-45-67","Total":"51000","Comment":"Payment within 5 business days after signing","UserName":"John Smith"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.document.getfields
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"values":{"DocumentNumber":"DG-2026-001","CurrentDate":"2026-03-18T00:00:00+02:00","ClientName":"Ltd. Chamomile","ClientPhone":"+1 999 123-45-67","Total":"51000","Comment":"Payment within 5 business days after signing","UserName":"John Smith"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.document.getfields
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.document.getfields',
          {
              id: 51,
              values: {
                  DocumentNumber: 'DG-2026-001',
                  CurrentDate: '2026-03-18T00:00:00+02:00',
                  ClientName: 'Ltd. Chamomile',
                  ClientPhone: '+1 999 123-45-67',
                  Total: '51000',
                  Comment: 'Payment within 5 business days after signing',
                  UserName: 'John Smith'
              }
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
          'documentgenerator.document.getfields',
          [
              'id' => 51,
              'values' => [
                  'DocumentNumber' => 'DG-2026-001',
                  'CurrentDate' => '2026-03-18T00:00:00+02:00',
                  'ClientName' => 'Ltd. Chamomile',
                  'ClientPhone' => '+1 999 123-45-67',
                  'Total' => '51000',
                  'Comment' => 'Payment within 5 business days after signing',
                  'UserName' => 'John Smith',
              ],
          ]
      );

      $result = $response->getResponseData()->getResult();
      print_r($result);
  } catch (Throwable $e) {
      echo $e->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'documentgenerator.document.getfields',
      {
          id: 51,
          values: {
              DocumentNumber: 'DG-2026-001',
              CurrentDate: '2026-03-18T00:00:00+02:00',
              ClientName: 'Ltd. Chamomile',
              ClientPhone: '+1 999 123-45-67',
              Total: '51000',
              Comment: 'Payment within 5 business days after signing',
              UserName: 'John Smith'
          }
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
      'documentgenerator.document.getfields',
      [
          'id' => 51,
          'values' => [
              'DocumentNumber' => 'DG-2026-001',
              'CurrentDate' => '2026-03-18T00:00:00+02:00',
              'ClientName' => 'Ltd. Chamomile',
              'ClientPhone' => '+1 999 123-45-67',
              'Total' => '51000',
              'Comment' => 'Payment within 5 business days after signing',
              'UserName' => 'John Smith',
          ],
      ]
  );

  print_r($result);
  ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "documentFields": {
            "DocumentNumber": {
                "title": "Number",
                "value": "DG-2026-001",
                "required": "Y",
                "group": [
                    "Document"
                ],
                "chain": "this.DOCUMENT.DOCUMENT_NUMBER",
                "default": "DG-2026-001"
            },
            "CurrentDate": {
                "value": "2026-03-18T00:00:00+02:00",
                "default": ""
            },
            "ClientName": {
                "value": "Ltd. Chamomile",
                "default": ""
            },
            "ClientPhone": {
                "value": "+1 999 123-45-67",
                "default": ""
            },
            "Total": {
                "value": "51000",
                "default": ""
            },
            "Comment": {
                "value": "Payment within 5 business days after signing",
                "default": ""
            },
            "UserName": {
                "value": "John Smith",
                "default": ""
            },
            "DocumentTitle": {
                "title": "Document Title",
                "value": "SUPPLY_CONTRACT Template 1773843147554 DG-2026-001",
                "group": [
                    "Document"
                ],
                "chain": [
                    {},
                    "getTitle"
                ],
                "required": "Y",
                "default": "SUPPLY_CONTRACT Template 1773843147554 DG-2026-001"
            }
        }
    },
    "time": {
        "start": 1773931419,
        "finish": 1773931419.57404,
        "duration": 0.5740399360656738,
        "processing": 0,
        "date_start": "2026-03-19T17:43:39+02:00",
        "date_finish": "2026-03-19T17:43:39+02:00",
        "operating_reset_at": 1773932019,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **documentFields**
[`object`](../data-types.md) | Set of document fields by field code [(detailed description)](#document-fields) ||
|#

#### DocumentFields Object {#document-fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../data-types.md) | Field name ||
|| **value**
[`string`](../data-types.md) | Current field value ||
|| **default**
[`string`](../data-types.md) | Base field value ||
|| **group**
[`array`](../data-types.md) | Field group in the template ||
|| **chain**
[`string`](../data-types.md) \| [`array`](../data-types.md) | Field chain, if available.

Can be a string (e.g., `this.DOCUMENT.DOCUMENT_NUMBER`) or an array (e.g., `[{}, "getTitle"]`) ||
|| **required**
[`string`](../data-types.md) | Field required indicator

Possible values:
- `Y` — field is required
- `N` — field is optional ||
|| **type**
[`string`](../data-types.md) | Field type, if specified ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "You do not have permissions to view documents"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Document constructor must be public | Required parameter `id` not provided ||
|| `400` | `0` | Document not found | Document with the specified `id` not found ||
|| `400` | `0` | You do not have permissions to view documents | Insufficient rights to modify the document ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-document-add.md)
- [{#T}](./document-generator-document-update.md)
- [{#T}](./document-generator-document-get.md)
- [{#T}](./document-generator-document-list.md)
- [{#T}](./document-generator-document-delete.md)
- [{#T}](./document-generator-document-enable-public-url.md)