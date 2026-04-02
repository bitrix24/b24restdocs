# Get the List of Templates documentgenerator.template.list

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.list` returns a list of templates based on the filter.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to return. Default is `["*"]`.

For `field_N`, use fields from the [fields table](#list-fields).

To retrieve access codes and providers, add `users` and `providers` fields to `select` ||
|| **order**
[`object`](../../data-types.md) | An object for sorting templates in the format `{"field_1":"value_1", ... "field_N":"value_N"}`.

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

For `field_N`, use fields from the [fields table](#list-fields).
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering templates in the format `{"field_1":"value_1", ... "field_N":"value_N"}`.

For `field_N`, use fields from the [fields table](#list-fields).

You can add a prefix to the keys `field_n` to specify the filter operation. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
  - `"mol%"` — searches for values starting with "mol"
  - `"%mol"` — searches for values ending with "mol"
  - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

By default, the filter `isDeleted = "N"` is applied ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N — 1) * 50`, where `N` is the desired page number ||
|#

### Fields for select, order, filter {#list-fields}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Template identifier ||
|| **active**
[`char`](../../data-types.md) | Template activity. Possible values:
- `Y` — active
- `N` — inactive
||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the template ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **sort**
[`integer`](../../data-types.md) | Sorting index ||
|| **createTime**
[`datetime`](../../data-types.md) | Creation date and time ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date and time ||
|| **numeratorId**
[`integer`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Use of stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **users**
[`array`](../../data-types.md) | Access codes for the template. Only for `select` ||
|| **providers**
[`array`](../../data-types.md) | Data providers for the template. Only for `select` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "select": [
        "*",
        "users",
        "providers"
      ],
      "order": {
        "id": "desc"
      },
      "filter": {
        "region": "de",
        "active": "Y",
        ">=createTime": "2026-03-18T00:00:00+01:00"
      },
      "start": 0
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/documentgenerator.template.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "select": [
        "*",
        "users",
        "providers"
      ],
      "order": {
        "id": "desc"
      },
      "filter": {
        "region": "de",
        "active": "Y",
        ">=createTime": "2026-03-18T00:00:00+01:00"
      },
      "start": 0,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/documentgenerator.template.list
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'documentgenerator.template.list',
          {
              select: ['*', 'users', 'providers'],
              order: {
                  id: 'desc'
              },
              filter: {
                  region: 'de',
                  active: 'Y',
                  '>=createTime': '2026-03-18T00:00:00+01:00'
              },
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
          'documentgenerator.template.list',
          [
              'select' => ['*', 'users', 'providers'],
              'order' => [
                  'id' => 'desc'
              ],
              'filter' => [
                  'region' => 'de',
                  'active' => 'Y',
                  '>=createTime' => '2026-03-18T00:00:00+01:00'
              ],
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
      'documentgenerator.template.list',
      {
          select: ['*', 'users', 'providers'],
          order: {
              id: 'desc'
          },
          filter: {
              region: 'de',
              active: 'Y',
              '>=createTime': '2026-03-18T00:00:00+01:00'
          },
          start: 0
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
      'documentgenerator.template.list',
      [
          'select' => ['*', 'users', 'providers'],
          'order' => [
              'id' => 'desc'
          ],
          'filter' => [
              'region' => 'de',
              'active' => 'Y',
              '>=createTime' => '2026-03-18T00:00:00+01:00'
          ],
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
        "templates": {
            "57": {
                "id": "57",
                "active": "Y",
                "name": "SUPPLY_CONTRACT_NEW Template",
                "code": "REST_TEMPLATE",
                "region": "de",
                "sort": "700",
                "createTime": "2026-03-23T16:51:25+01:00",
                "updateTime": "2026-03-23T17:26:39+01:00",
                "createdBy": "503",
                "updatedBy": "503",
                "moduleId": "rest",
                "fileId": "5641",
                "bodyType": "Bitrix\\DocumentGenerator\\Body\\Docx",
                "numeratorId": "3",
                "withStamps": "N",
                "productsTableVariant": "",
                "isDeleted": "N",
                "isDefault": "N",
                "download": "/bitrix/services/main/ajax.php?action=documentgenerator.api.template.download&SITE_ID=s1&id=57",
                "providers": [
                    "bitrix\\documentgenerator\\dataprovider\\rest"
                ],
                "users": [
                    "U503"
                ],
                "downloadMachine": "https://mysite.com/rest/documentgenerator.api.template.download.json?auth=3f5cc2690000071b00000844000001f7f0f107506cefda4aa5b9d0dc80371e7f0f7e26&token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTU3Jl89RFB3MHZYN1A3RXdiMm5EenY2VFYyc1Vkb0hPejA3SlM%3D%7CImRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZkb2N1bWVudGdlbmVyYXRvcnxZV04wYVc5dVBXUnZZM1Z0Wlc1MFoyVnVaWEpoZEc5eUxtRndhUzUwWlcxd2JHRjBaUzVrYjNkdWJHOWhaQ1pUU1ZSRlgwbEVQWE14Sm1sa1BUVTNKbDg5UkZCM01IWllOMUEzUlhkaU1tNUVlblkyVkZZeWMxVmtiMGhQZWpBM1NsTT18M2Y1Y2MyNjkwMDAwMDcxYjAwMDAwODQ0MDAwMDAxZjdmMGYxMDc1MDZjZWZkYTRhYTViOWQwZGM4MDM3MWU3ZjBmN2UyNiI%3D.XuC83kQJE21vRoqv1%2FgdT8OiYq4JdpiSLxumy%2F1UcB0%3D"
            },
            "53": {
                "id": "53", // description of the next template
                ...
            },
            "51": {
                "id": "51", // description of the next template
                ...
            }
        }
    },
    "total": 3,
    "time": {
        "start": 1774341679,
        "finish": 1774341679.641056,
        "duration": 0.6410560607910156,
        "processing": 0,
        "date_start": "2026-03-24T11:41:19+01:00",
        "date_finish": "2026-03-24T11:41:19+01:00",
        "operating_reset_at": 1774342279,
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
[`integer`](../../data-types.md) | Total number of templates in the list ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **templates**
[`object`](../../data-types.md) | Object of templates, where the key is `id`, and the value is the template data [(detailed description)](#template) ||
|#

#### Template Object Element {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Template identifier ||
|| **active**
[`char`](../../data-types.md) | Template activity. Possible values:
- `Y` — active
- `N` — inactive
||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the template ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **sort**
[`string`](../../data-types.md) | Sorting index ||
|| **createTime**
[`datetime`](../../data-types.md) | Creation date and time ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date and time ||
|| **createdBy**
[`string`](../../data-types.md) | Identifier of the user who created the template ||
|| **updatedBy**
[`string`](../../data-types.md) | Identifier of the user who updated the template ||
|| **moduleId**
[`string`](../../data-types.md) | Module identifier ||
|| **fileId**
[`string`](../../data-types.md) | Template file identifier ||
|| **bodyType**
[`string`](../../data-types.md) | Class of the body type of the template ||
|| **numeratorId**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Use of stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **productsTableVariant**
[`string`](../../data-types.md) | Variant of the products table ||
|| **isDeleted**
[`char`](../../data-types.md) | Deletion flag of the template. Possible values:
- `Y` — deleted
- `N` — not deleted
||
|| **isDefault**
[`char`](../../data-types.md) | Default template flag. Possible values:
- `Y` — yes
- `N` — no
||
|| **download**
[`string`](../../data-types.md) | Link to download the template ||
|| **downloadMachine**
[`string`](../../data-types.md) | Link to download the template for machine processing ||
|| **users**
[`array`](../../data-types.md) | Array of access codes if the field is selected in `select` ||
|| **providers**
[`array`](../../data-types.md) | Array of providers if the field is selected in `select` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "You do not have permissions to modify templates"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient rights to retrieve the list of templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-delete.md)
- [{#T}](./document-generator-template-get-fields.md)