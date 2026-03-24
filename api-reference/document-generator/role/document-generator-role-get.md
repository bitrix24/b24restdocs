# Get Role by ID documentgenerator.role.get

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify document generator settings

The method `documentgenerator.role.get` returns information about the role and its access permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Role identifier.

You can obtain the identifier after [creating a role](./document-generator-role-add.md) or by using the [get role list method](./document-generator-role-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.role.get
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.role.get
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.role.get',
          {
              id: 9
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
          'documentgenerator.role.get',
          [
              'id' => 9,
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
      'documentgenerator.role.get',
      {
          id: 9
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
      'documentgenerator.role.get',
      [
          'id' => 9,
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
        "role": {
            "id": 9,
            "name": "Template Editors",
            "code": "DOCGEN_TEMPLATE_EDITORS",
            "permissions": {
                "settings": {
                    "modify": ""
                },
                "templates": {
                    "modify": "A"
                },
                "documents": {
                    "modify": "X",
                    "view": "X"
                }
            }
        }
    },
    "time": {
        "start": 1774014973,
        "finish": 1774014974.093322,
        "duration": 1.0933220386505127,
        "processing": 0,
        "date_start": "2026-03-20T16:56:13+02:00",
        "date_finish": "2026-03-20T16:56:14+02:00",
        "operating_reset_at": 1774015574,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **role**
[`object`](../../data-types.md) | Role data [(detailed description)](#role) ||
|#

#### Role Object {#role}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Role identifier ||
|| **name**
[`string`](../../data-types.md) | Role name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the role ||
|| **permissions**
[`object`](../../data-types.md) | Role permissions [(detailed description)](#permissions) ||
|#

#### Permissions Object {#permissions}

#|
|| **Name**
`type` | **Description** ||
|| **settings**
[`object`](../../data-types.md) | Permissions for settings [(detailed description)](#permissions-settings) ||
|| **templates**
[`object`](../../data-types.md) | Permissions for templates [(detailed description)](#permissions-templates) ||
|| **documents**
[`object`](../../data-types.md) | Permissions for documents [(detailed description)](#permissions-documents) ||
|#

#### Settings Object {#permissions-settings}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to settings. Possible values:
- `""` — no access
- `X` — full access
||
|#

#### Templates Object {#permissions-templates}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to templates. Possible values:
- `""` — no access
- `A` — only own items
- `D` — own and department colleagues
- `X` — full access
 ||
|#

#### Documents Object {#permissions-documents}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to modify documents. Possible values:
- `""` — no access
- `X` — full access
||
|| **view**
[`string`](../../data-types.md) | Access level to view documents. Possible values:
- `""` — no access
- `X` — full access
||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "0",
    "error_description": "You do not have permissions to modify settings"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Bitrix\DocumentGenerator\Model\Role All parameters in the constructor must have real class type | Required parameter `id` not provided ||
|| `400` | `100` | Could not construct parameter {role} | Non-existent parameter `id` provided ||
|| `400` | `0` | You do not have permissions to modify settings | Insufficient permissions to modify document generator settings ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Your plan does not support this operation | The plan does not support the function of permission delimitation for the document generator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-role-add.md)
- [{#T}](./document-generator-role-update.md)
- [{#T}](./document-generator-role-list.md)
- [{#T}](./document-generator-role-delete.md)
- [{#T}](./document-generator-role-fill-accesses.md)