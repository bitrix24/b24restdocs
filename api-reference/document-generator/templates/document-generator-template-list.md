# Get a List of documentgenerator.template.list Templates

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates

The method `documentgenerator.template.list` returns a list of templates based on the filter.

{% note info "" %}

The method returns only templates of the `rest` module, that is, templates created with [documentgenerator.template.add](./document-generator-template-add.md). Templates of other modules, such as CRM, are not included in the list

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to return. Use fields from the [fields table](#list-fields).

Default is `["*"]`: all template fields are returned except `users` and `providers`. To retrieve access codes and providers, add the `users` and `providers` fields to `select`, for example `["*", "users", "providers"]`.

The `id`, `download`, and `downloadMachine` fields are always returned, even if they are not listed in `select` ||
|| **order**
[`object`](../../data-types.md) | An object for sorting templates in the format `{"field_1":"value_1", ... "field_N":"value_N"}`. You can pass several fields; sorting is applied in the order they are listed.

For `field_N`, use fields from the [fields table](#list-fields), except `users` and `providers`.

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

No sorting is applied by default ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering templates in the format `{"field_1":"value_1", ... "field_N":"value_N"}`.

For `field_N`, use fields from the [fields table](#list-fields), except `users` and `providers`. Pass the values of the `createTime` and `updateTime` fields in the [`datetime`](../../data-types.md) format, for example `2026-03-18T00:00:00+01:00`.

You can add a prefix to the `field_n` keys to specify the filter operation. Possible prefix values:
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

By default, the method returns only templates that are not marked as deleted: the filter `isDeleted = "N"` is applied. To retrieve templates marked as deleted, pass `"isDeleted": "Y"`; to retrieve all templates, pass `"@isDeleted": ["Y", "N"]`.

A filter by `moduleId` is ignored: the method always selects templates of the `rest` module ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size for results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N — 1) * 50`, where `N` is the desired page number.

A value that is not a multiple of 50 is rounded down to the nearest multiple of 50: for example, `start` = `30` returns the first page.

Default is `0` ||
|#

{% note warning "" %}

If you pass a field in `select`, `order`, or `filter` that is not in the [fields table](#list-fields), the method returns error `100` with the text `Internal server error.`

{% endnote %}

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
[`string`](../../data-types.md) | Template symbolic code ||
|| **region**
[`string`](../../data-types.md) | Template region, for example `de`. You can retrieve the list of regions with the [documentgenerator.region.list](../region/document-generator-region-list.md) method ||
|| **timezone**
[`string`](../../data-types.md) | Template time zone in IANA format, for example `Europe/Berlin` ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **createTime**
[`datetime`](../../data-types.md) | Creation date and time ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date and time ||
|| **createdBy**
[`user`](../../data-types.md#standart-objects) | Identifier of the user who created the template ||
|| **updatedBy**
[`user`](../../data-types.md#standart-objects) | Identifier of the user who updated the template ||
|| **moduleId**
[`string`](../../data-types.md) | Template module identifier. Always `rest` for templates created via the REST API ||
|| **fileId**
[`integer`](../../data-types.md) | Template file identifier ||
|| **bodyType**
[`string`](../../data-types.md) | Class of the template body type, for example `Bitrix\DocumentGenerator\Body\Docx` ||
|| **numeratorId**
[`integer`](../../data-types.md) | Identifier of the numerator. You can retrieve the list of numerators with the [documentgenerator.numerator.list](../numerators/document-generator-numerator-list.md) method ||
|| **withStamps**
[`char`](../../data-types.md) | Use of stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **productsTableVariant**
[`string`](../../data-types.md) | Variant of the products table. Possible values:
- empty string — products and services
- `goods` — products only
- `service` — services only
||
|| **isDeleted**
[`char`](../../data-types.md) | Deletion flag of the template. Possible values:
- `Y` — the template is marked as deleted
- `N` — the template is not deleted
||
|| **isDefault**
[`char`](../../data-types.md) | Default template flag. Possible values:
- `Y` — yes
- `N` — no
||
|| **users**
[`array`](../../data-types.md) | Access codes for the template. Only for `select`. Possible values:
- `U{id}` — user
- `G{id}` — user group
- `AU` — all authorized users
- `UA` — all users
- `D{id}` — department
- `DR{id}` — department with subdepartments
- `SG{id}` — workgroup or project
- `SG{id}_A` — owner of the workgroup or project
- `SG{id}_E` — moderators of the workgroup or project
- `SG{id}_K` — all members of the workgroup or project
||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TemplateItem = {
      id: string
      active: string
      name: string
      code: string
      region: string
      timezone: string | null
      sort: string
      createTime: ISODate | null
      updateTime: ISODate | null
      createdBy: string
      updatedBy: string | null
      moduleId: string
      fileId: string
      bodyType: string
      numeratorId: string
      withStamps: string
      productsTableVariant: string
      isDeleted: string
      isDefault: string
      download: string
      downloadMachine: string
      users?: string[]
      providers?: string[]
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    // When nothing matches the filter, templates is an empty array []
    type TemplateListResult = {
      templates: Record<string, TemplateItem> | []
    }

    try {
      // documentgenerator.template.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<TemplateListResult>({
        method: 'documentgenerator.template.list',
        params: {
          select: ['*', 'users', 'providers'],
          order: {
            id: 'desc',
          },
          filter: {
            region: 'de',
            active: 'Y',
            '>=createTime': '2026-03-18T00:00:00+01:00',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Templates count:', Object.keys(result.templates).length, result.templates)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listTemplates() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // documentgenerator.template.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'documentgenerator.template.list',
            params: {
              select: ['*', 'users', 'providers'],
              order: {
                id: 'desc',
              },
              filter: {
                region: 'de',
                active: 'Y',
                '>=createTime': '2026-03-18T00:00:00+01:00',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Templates count:', Object.keys(result.templates).length, result.templates)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplates)
    </script>
    ```

- Python

  ```python
  from b24pysdk.errors import BitrixAPIError, BitrixSDKException

  try:
      bitrix_response = client.documentgenerator.template.list(
          select=[
              "*",
              "users",
              "providers",
          ],
          order={
              "id": "desc",
          },
          filter={
              "region": "de",
              "active": "Y",
              ">=createTime": "2026-03-18T00:00:00+01:00",
          },
          start=0,
      ).response
      result = bitrix_response.result
      print(result)
  except BitrixAPIError as error:
      print(
          "Bitrix API error",
          f"error: {error.error}",
          f"error_description: {error.error_description}",
          sep="\n",
      )
  except BitrixSDKException as error:
      print(f"Bitrix SDK error: {error.message}")
  except Exception as error:
      print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "documentgenerator.template.list", b24.Params{
    	"select": []string{"*", "users", "providers"},
    	"order": b24.Params{
    		"id": "desc",
    	},
    	"filter": b24.Params{
    		"region":       "de",
    		"active":       "Y",
    		">=createTime": "2026-03-18T00:00:00+01:00",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("documentgenerator.template.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "templates": {
            "387": {
                "id": "387",
                "active": "Y",
                "name": "Supply contract",
                "code": "SUPPLY_CONTRACT",
                "region": "de",
                "timezone": "Europe/Berlin",
                "sort": "700",
                "createTime": "2026-11-11T13:42:08+01:00",
                "updateTime": "2026-11-11T13:42:08+01:00",
                "createdBy": "1269",
                "updatedBy": null,
                "moduleId": "rest",
                "fileId": "6629",
                "bodyType": "Bitrix\\DocumentGenerator\\Body\\Docx",
                "numeratorId": "3",
                "withStamps": "N",
                "productsTableVariant": "",
                "isDeleted": "N",
                "isDefault": "N",
                "download": "/bitrix/services/main/ajax.php?action=documentgenerator.api.template.download&SITE_ID=s1&id=387",
                "providers": [
                    "bitrix\\documentgenerator\\dataprovider\\rest"
                ],
                "users": [
                    "U1269"
                ],
                "downloadMachine": "https://mysite.bitrix24.com/rest/1269/webhook_code/documentgenerator.api.template.download/?token=documentgenerator%7CYWN0aW9uPWRvY3VtZW50Z2VuZXJhdG9yLmFwaS50ZW1wbGF0ZS5kb3dubG9hZCZTSVRFX0lEPXMxJmlkPTM4NyZfPXYyRWc2U0cyYXBrVHZTZXloazNMSnB2VVd4UEZpdnlJ%7C..."
            }
        }
    },
    "total": 1,
    "time": {
        "start": 1794393731,
        "finish": 1794393731.550351,
        "duration": 0.5503509044647217,
        "processing": 0,
        "date_start": "2026-11-11T13:42:11+01:00",
        "date_finish": "2026-11-11T13:42:11+01:00",
        "operating_reset_at": 1794394320,
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
|| **next**
[`integer`](../../data-types.md) | The `start` value for retrieving the next page. Returned if there are more templates after the current page ||
|| **total**
[`integer`](../../data-types.md) | Total number of templates that match the filter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **templates**
[`object`](../../data-types.md) | Object of templates, where the key is `id`, and the value is the template data [(detailed description)](#template).

If nothing matches the filter, an empty array `[]` is returned ||
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
[`string`](../../data-types.md) | Template symbolic code ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **timezone**
[`string`](../../data-types.md) | Template time zone in IANA format, for example `Europe/Berlin`.

`null` means the template has no time zone of its own: dates are displayed in the shared time zone set in the document template settings, or in the server time zone if none is selected ||
|| **sort**
[`string`](../../data-types.md) | Sort index ||
|| **createTime**
[`datetime`](../../data-types.md) | Creation date and time ||
|| **updateTime**
[`datetime`](../../data-types.md) | Update date and time ||
|| **createdBy**
[`string`](../../data-types.md) | Identifier of the user who created the template ||
|| **updatedBy**
[`string`](../../data-types.md) | Identifier of the user who updated the template. `null` means the template has not been updated since it was created ||
|| **moduleId**
[`string`](../../data-types.md) | Module identifier, always `rest` ||
|| **fileId**
[`string`](../../data-types.md) | Template file identifier. For a template marked as deleted, the value is `0` ||
|| **bodyType**
[`string`](../../data-types.md) | Class of the template body type, for example `Bitrix\DocumentGenerator\Body\Docx` ||
|| **numeratorId**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Use of stamps and signatures. Possible values:
- `Y` — yes
- `N` — no
||
|| **productsTableVariant**
[`string`](../../data-types.md) | Variant of the products table. Possible values:
- empty string — products and services
- `goods` — products only
- `service` — services only
||
|| **isDeleted**
[`char`](../../data-types.md) | Deletion flag of the template. Possible values:
- `Y` — the template is marked as deleted because documents have been created from it
- `N` — the template is not deleted
||
|| **isDefault**
[`char`](../../data-types.md) | Default template flag. Possible values:
- `Y` — yes
- `N` — no
||
|| **download**
[`string`](../../data-types.md) | Relative link for downloading the template file in a browser. Always returned ||
|| **downloadMachine**
[`string`](../../data-types.md) | Absolute link for downloading the template file via the REST API with an access token. Always returned ||
|| **users**
[`array`](../../data-types.md) | Array of access codes, for example `["U1269"]`. Returned if the field is selected in `select` ||
|| **providers**
[`array`](../../data-types.md) | Array of data providers in lowercase, for example `["bitrix\\documentgenerator\\dataprovider\\rest"]`. Returned if the field is selected in `select` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Internal server error."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Internal server error. | A field that is not in the [fields table](#list-fields) was passed in `select`, `order`, or `filter` ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to work with document generator templates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-generator-template-add.md)
- [{#T}](./document-generator-template-update.md)
- [{#T}](./document-generator-template-get.md)
- [{#T}](./document-generator-template-delete.md)
- [{#T}](./document-generator-template-get-fields.md)