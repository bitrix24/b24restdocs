# Get a List of Roles documentgenerator.role.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator settings

The method `documentgenerator.role.list` returns a list of roles without detailing permissions.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size for results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N — 1) * 50`, where `N` is the desired page number ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.role.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.role.list
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.role.list',
          {
              start: 0
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
          'documentgenerator.role.list',
          [
              'start' => 0,
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
      'documentgenerator.role.list',
      {},
      function(result)
      {
          if (result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.log(result.data());

              if (result.more())
              {
                  result.next();
              }
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'documentgenerator.role.list',
      [
          'start' => 0,
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
        "roles": [
            {
                "id": 1,
                "name": "Administrator",
                "code": "ADMIN"
            },
            {
                "id": 3,
                "name": "Manager",
                "code": "MANAGER"
            },
            {
                "id": 9,
                "name": "Template Editors",
                "code": "DOCGEN_TEMPLATE_EDITORS"
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1774015085,
        "finish": 1774015085.612552,
        "duration": 0.6125519275665283,
        "processing": 0,
        "date_start": "2026-03-20T16:58:05+01:00",
        "date_finish": "2026-03-20T16:58:05+01:00",
        "operating_reset_at": 1774015685,
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
|| **total**
[`integer`](../../data-types.md) | Number of roles in the current selection ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **roles**
[`array`](../../data-types.md) | List of roles [(detailed description)](#roles) ||
|#

#### Role Array Element {#roles}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Role identifier ||
|| **name**
[`string`](../../data-types.md) | Role name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the role ||
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
|| `400` | `0` | You do not have permissions to modify settings | Insufficient rights to modify document generator settings ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Your plan does not support this operation | The plan does not allow for permission segregation for the document generator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-role-add.md)
- [{#T}](./document-generator-role-update.md)
- [{#T}](./document-generator-role-get.md)
- [{#T}](./document-generator-role-delete.md)
- [{#T}](./document-generator-role-fill-accesses.md)