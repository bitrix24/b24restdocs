# Bind Users to Roles documentgenerator.role.fillaccesses

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator settings

The method `documentgenerator.role.fillaccesses` completely overwrites the mapping of roles to access codes.

{% note warning "" %}

The method fully overwrites the settings.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **accesses**
[`array`](../../data-types.md) | An array of role bindings to access codes [(detailed description)](#accesses) ||
|#

#### Array Element accesses {#accesses}

#|
|| **Name**
`type` | **Description** ||
|| **roleId***
[`integer`](../../data-types.md) | Role identifier.

You can obtain the identifier after [creating a role](./document-generator-role-add.md) or by using the [get list of roles](./document-generator-role-list.md) method ||
|| **accessCode***
[`string`](../../data-types.md) | Access code for a user, group, or department.

Possible values:

- `U{id}` — user
- `G{id}` — user group
- `AU` — all authorized users
- `UA` — all users
- `D{id}` — department
- `DR{id}` — department with sub-departments
- `SG{id}` — workgroup or project
- `SG{id}_A` — owner of the workgroup or project
- `SG{id}_E` — moderators of the workgroup or project
- `SG{id}_K` — all members of the workgroup or project
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "accesses": [
        {
          "roleId": 9,
          "accessCode": "U1"
        },
        {
          "roleId": 9,
          "accessCode": "D1"
        },
        {
          "roleId": 9,
          "accessCode": "UA"
        }
      ]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.role.fillaccesses
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "accesses": [
        {
          "roleId": 9,
          "accessCode": "U1"
        },
        {
          "roleId": 9,
          "accessCode": "D1"
        },
        {
          "roleId": 9,
          "accessCode": "UA"
        }
      ],
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.role.fillaccesses
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.role.fillaccesses',
          {
              accesses: [
                  {
                      roleId: 9,
                      accessCode: 'U1'
                  },
                  {
                      roleId: 9,
                      accessCode: 'D1'
                  },
                  {
                      roleId: 9,
                      accessCode: 'UA'
                  }
              ]
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
          'documentgenerator.role.fillaccesses',
          [
              'accesses' => [
                  [
                      'roleId' => 9,
                      'accessCode' => 'U1',
                  ],
                  [
                      'roleId' => 9,
                      'accessCode' => 'D1',
                  ],
                  [
                      'roleId' => 9,
                      'accessCode' => 'UA',
                  ],
              ],
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
      'documentgenerator.role.fillaccesses',
      {
          accesses: [
              {
                  roleId: 9,
                  accessCode: 'U1'
              },
              {
                  roleId: 9,
                  accessCode: 'D1'
              },
              {
                  roleId: 9,
                  accessCode: 'UA'
              }
          ]
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
      'documentgenerator.role.fillaccesses',
      [
          'accesses' => [
              [
                  'roleId' => 9,
                  'accessCode' => 'U1',
              ],
              [
                  'roleId' => 9,
                  'accessCode' => 'D1',
              ],
              [
                  'roleId' => 9,
                  'accessCode' => 'UA',
              ],
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
    "result": null,
    "time": {
        "start": 1773914210,
        "finish": 1773914211.747039,
        "duration": 1.7470390796661377,
        "processing": 1,
        "date_start": "2026-03-19T12:56:50+02:00",
        "date_finish": "2026-03-19T12:56:51+02:00",
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
`null` | The method applies the new bindings and returns `null`.

Also returns `null` without errors:
- if the required parameter `accesses` is not provided 
- if a non-existent `roleId` is provided
- if a non-existent `accessCode` is provided 
||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `400` | `BX_EMPTY_REQUIRED` | Required field "ROLE_ID" is not filled | In one of the `accesses` elements, `roleId` is not provided ||
|| `400` | `BX_EMPTY_REQUIRED` | Required field "ACCESS_CODE" is not filled | In one of the `accesses` elements, `accessCode` is not provided ||
|| `400` | `0` | You do not have permissions to modify settings | Insufficient rights to modify document generator settings ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Your plan does not support this operation | The plan does not allow for permission differentiation for the document generator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-role-add.md)
- [{#T}](./document-generator-role-update.md)
- [{#T}](./document-generator-role-get.md)
- [{#T}](./document-generator-role-list.md)
- [{#T}](./document-generator-role-delete.md)