# Add Role documentgenerator.role.add

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify document generator settings

The method `documentgenerator.role.add` adds a new role.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Role fields [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Role name ||
|| **code**
[`string`](../../data-types.md) | Symbolic role code ||
|| **permissions**
[`object`](../../data-types.md) | Role permissions [(detailed description)](#permissions).

{% note warning "" %}

Keys within `permissions` are case-sensitive. Only pass them in uppercase:
- `SETTINGS`, `TEMPLATES`, `DOCUMENTS`
- `MODIFY`, `VIEW`

If keys are passed in lowercase, the role will be saved, but permissions will be empty.

{% endnote %}
||
|#

#### Object permissions {#permissions}

#|
|| **Name**
`type` | **Description** ||
|| **SETTINGS**
[`object`](../../data-types.md) | Permissions for settings [(detailed description)](#permissions-settings) ||
|| **TEMPLATES**
[`object`](../../data-types.md) | Permissions for templates [(detailed description)](#permissions-templates) ||
|| **DOCUMENTS**
[`object`](../../data-types.md) | Permissions for documents [(detailed description)](#permissions-documents) ||
|#

#### Object settings {#permissions-settings}

#|
|| **Name**
`type` | **Description** ||
|| **MODIFY**
[`string`](../../data-types.md) | Access level to settings. Possible values:
- `""` — no access
- `X` — full access
||
|#

#### Object templates {#permissions-templates}

#|
|| **Name**
`type` | **Description** ||
|| **MODIFY**
[`string`](../../data-types.md) | Access level to templates. Possible values:
- `""` — no access
- `A` — only own items
- `D` — own and department colleagues' items
- `X` — full access
||
|#

#### Object documents {#permissions-documents}

#|
|| **Name**
`type` | **Description** ||
|| **MODIFY**
[`string`](../../data-types.md) | Access level for modifying documents. Possible values:
- `""` — no access
- `X` — full access
||
|| **VIEW**
[`string`](../../data-types.md) | Access level for viewing documents. Possible values:
- `""` — no access
- `X` — full access
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
      "fields": {
        "name": "Template Editors",
        "code": "DOCGEN_TEMPLATE_EDITORS",
        "permissions": {
          "SETTINGS": {
            "MODIFY": ""
          },
          "TEMPLATES": {
            "MODIFY": "D"
          },
          "DOCUMENTS": {
            "MODIFY": "X",
            "VIEW": "X"
          }
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.role.add
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "name": "Template Editors",
        "code": "DOCGEN_TEMPLATE_EDITORS",
        "permissions": {
          "SETTINGS": {
            "MODIFY": ""
          },
          "TEMPLATES": {
            "MODIFY": "D"
          },
          "DOCUMENTS": {
            "MODIFY": "X",
            "VIEW": "X"
          }
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.role.add
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.role.add',
          {
              fields: {
                  name: 'Template Editors',
                  code: 'DOCGEN_TEMPLATE_EDITORS',
                  permissions: {
                      SETTINGS: {
                          MODIFY: ''
                      },
                      TEMPLATES: {
                          MODIFY: 'D'
                      },
                      DOCUMENTS: {
                          MODIFY: 'X',
                          VIEW: 'X'
                      }
                  }
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
          'documentgenerator.role.add',
          [
              'fields' => [
                  'name' => 'Template Editors',
                  'code' => 'DOCGEN_TEMPLATE_EDITORS',
                  'permissions' => [
                      'SETTINGS' => [
                          'MODIFY' => '',
                      ],
                      'TEMPLATES' => [
                          'MODIFY' => 'D',
                      ],
                      'DOCUMENTS' => [
                          'MODIFY' => 'X',
                          'VIEW' => 'X',
                      ],
                  ],
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
      'documentgenerator.role.add',
      {
          fields: {
              name: 'Template Editors',
              code: 'DOCGEN_TEMPLATE_EDITORS',
              permissions: {
                  SETTINGS: {
                      MODIFY: ''
                  },
                  TEMPLATES: {
                      MODIFY: 'D'
                  },
                  DOCUMENTS: {
                      MODIFY: 'X',
                      VIEW: 'X'
                  }
              }
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
      'documentgenerator.role.add',
      [
          'fields' => [
              'name' => 'Template Editors',
              'code' => 'DOCGEN_TEMPLATE_EDITORS',
              'permissions' => [
                  'SETTINGS' => [
                      'MODIFY' => '',
                  ],
                  'TEMPLATES' => [
                      'MODIFY' => 'D',
                  ],
                  'DOCUMENTS' => [
                      'MODIFY' => 'X',
                      'VIEW' => 'X',
                  ],
              ],
          ],
      ]
  );

  print_r($result);
  ```

{% endlist %}

## Response Handling

HTTP status: **200**

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
                    "modify": "D"
                },
                "documents": {
                    "modify": "X",
                    "view": "X"
                }
            }
        }
    },
    "time": {
        "start": 1774008850,
        "finish": 1774008850.528288,
        "duration": 0.5282878875732422,
        "processing": 0,
        "date_start": "2026-03-20T15:14:10+02:00",
        "date_finish": "2026-03-20T15:14:10+02:00",
        "operating_reset_at": 1774009450,
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

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **role**
[`object`](../../data-types.md) | Data of the created role [(detailed description)](#result-role) ||
|#

#### Object role {#result-role}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Role identifier ||
|| **name**
[`string`](../../data-types.md) | Role name ||
|| **code**
[`string`](../../data-types.md) | Symbolic role code ||
|| **permissions**
[`object`](../../data-types.md) | Role permissions [(detailed description)](#result-role-permissions) ||
|#

#### Object permissions {#result-role-permissions}

#|
|| **Name**
`type` | **Description** ||
|| **settings**
[`object`](../../data-types.md) | Permissions for settings [(detailed description)](#result-role-permissions-settings) ||
|| **templates**
[`object`](../../data-types.md) | Permissions for templates [(detailed description)](#result-role-permissions-templates) ||
|| **documents**
[`object`](../../data-types.md) | Permissions for documents [(detailed description)](#result-role-permissions-documents) ||
|#

#### Object settings {#result-role-permissions-settings}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to settings
Possible values:
- `""` — no access
- `X` — full access ||
|#

#### Object templates {#result-role-permissions-templates}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level to templates
Possible values:
- `""` — no access
- `A` — only own items
- `D` — own and department colleagues' items
- `X` — full access ||
|#

#### Object documents {#result-role-permissions-documents}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`string`](../../data-types.md) | Access level for modifying documents
Possible values:
- `""` — no access
- `X` — full access ||
|| **view**
[`string`](../../data-types.md) | Access level for viewing documents
Possible values:
- `""` — no access
- `X` — full access ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {fields}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` was not provided or was empty ||
|| `400` | `BX_EMPTY_REQUIRED` | Required field "NAME" is not filled | Required field `name` was not provided in the `fields` parameter ||
|| `400` | `0` | You do not have permissions to modify settings | Insufficient permissions to modify document generator settings ||
|| `403` | `DOCGEN_ACCESS_ERROR` | Your plan does not support this operation | The plan does not support the function of permission management for the document generator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-role-update.md)
- [{#T}](./document-generator-role-get.md)
- [{#T}](./document-generator-role-list.md)
- [{#T}](./document-generator-role-delete.md)
- [{#T}](./document-generator-role-fill-accesses.md)