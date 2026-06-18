# Get a List of Document Templates crm.documentgenerator.template.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for document generator templates

The method `crm.documentgenerator.template.list` returns a list of document templates.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | A list of fields to return for the templates.

You can use:
- `'*'` — to select all standard fields of the template
- an explicit list of fields, for example `["id","name","region","active"]`

Additionally, the following are supported:
- `entityTypeId` — an array of template bindings to CRM entities
- `users` — an array of access permission codes

Main fields for `select`: `id`, `name`, `region`, `code`, `active`, `moduleId`, `numeratorId`, `withStamps`, `isDeleted`, `sort`, `createTime`, `updateTime`

See the list of template fields in the [`Type template`](#template) section. By default, `["*"]` is used. ||
|| **filter**
[`object`](../../data-types.md) | An object in the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — the name of the field for filtering
- `value_n` — the filter value

You can add prefixes to the keys `field_n`:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `=` — equal (default)
- `!=` or `!` — not equal

Features:
- if `isDeleted` is not provided, the filter `isDeleted = "N"` is applied
- the `moduleId` filter is forcibly limited to the value `crm`
- you can filter by `entityTypeId`, for example `["2","2_category_37"]`

Main fields for `filter`: `id`, `name`, `region`, `code`, `active`, `moduleId`, `numeratorId`, `withStamps`, `isDeleted`, `sort`, `createTime`, `updateTime`, `entityTypeId` ||
|| **order**
[`object`](../../data-types.md) | An object in the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — the name of the sorting field
- `value_n` — the sorting direction: `ASC` or `DESC`

Main fields for `order`: `id`, `name`, `region`, `code`, `active`, `moduleId`, `numeratorId`, `withStamps`, `isDeleted`, `sort`, `createTime`, `updateTime`

Example: `{"id":"DESC","sort":"ASC"}` ||
|| **start**
[`integer`](../../data-types.md) | Pagination parameter.

The page size is fixed: `50` records.

The formula for obtaining the N-th page:
`start = (N - 1) * 50`

More details in the article [Features of List Methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

Example of retrieving a list of templates where:
- fields `id`, `name`, `region`, `entityTypeId`, `users` are selected
- sorting by `id` in descending order
- filtering by region `de` and activity `Y`
- starting offset — `0`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","region","entityTypeId","users"],"order":{"id":"desc"},"filter":{"region":"de","active":"Y"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.template.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","region","entityTypeId","users"],"order":{"id":"desc"},"filter":{"region":"de","active":"Y"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.template.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type Template = {
      id: string
      name: string
      region: string
      download: string
      users: string[]
      entityTypeId: string[]
      downloadMachine: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplatesResult = {
      templates: Record<string, Template>
    }

    try {
      // crm.documentgenerator.template.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<TemplatesResult>({
        method: 'crm.documentgenerator.template.list',
        params: {
          select: ['id', 'name', 'region', 'entityTypeId', 'users'],
          order: { id: 'desc' },
          filter: { region: 'ru', active: 'Y' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Templates on this page:', Object.keys(result.templates).length)
        console.info(result.templates)
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
      async function fetchTemplateList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.documentgenerator.template.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.template.list',
            params: {
              select: ['id', 'name', 'region', 'entityTypeId', 'users'],
              order: { id: 'desc' },
              filter: { region: 'ru', active: 'Y' },
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
          console.info('Templates on this page:', Object.keys(result.templates).length)
          console.info(result.templates)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchTemplateList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.template.list',
                [
                    'select' => ['id', 'name', 'region', 'entityTypeId', 'users'],
                    'order' => ['id' => 'desc'],
                    'filter' => ['region' => 'de', 'active' => 'Y'],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting templates list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.template.list',
        {
            select: ['id', 'name', 'region', 'entityTypeId', 'users'],
            order: { id: 'desc' },
            filter: { region: 'de', active: 'Y' },
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());

            if (result.more()) {
                result.next();
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.template.list',
        [
            'select' => ['id', 'name', 'region', 'entityTypeId', 'users'],
            'order' => ['id' => 'desc'],
            'filter' => ['region' => 'de', 'active' => 'Y'],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "templates": {
            "39": {
                "id": "39",
                "name": "Demo Product Implementation",
                "region": "de",
                "download": "https://mysite.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=39",
                "users": [
                    "UA"
                ],
                "entityTypeId": [
                    "2_category_0",
                    "2_category_32"
                ],
                "downloadMachine": "https://mysite.com/rest/crm.documentgenerator.template.download.json?auth=***&token=***"
            },
            "37": {
                "id": "37",
                "name": "Goods Write-off Act (Germany)",
                "region": "de",
                "download": "https://mysite.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=37",
                "users": [
                    "UA"
                ],
                "entityTypeId": [
                    "2_category_37",
                    "bitrix\\crm\\integration\\documentgenerator\\dataprovider\\storedocumentdeduct"
                ],
                "downloadMachine": "https://mysite.com/rest/crm.documentgenerator.template.download.json?auth=***&token=***"
            }
        }
    },
    "total": 20,
    "time": {
        "start": 1773845479,
        "finish": 1773845479.829607,
        "duration": 0.8296070098876953,
        "processing": 0,
        "date_start": "2026-03-18T17:51:19+01:00",
        "date_finish": "2026-03-18T17:51:19+01:00",
        "operating_reset_at": 1773846079,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response. Contains the object [`templates`](#templates) ||
|| **total**
[`integer`](../../data-types.md) | The total number of templates matching the filter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object templates {#templates}

[`object`](../../data-types.md), where the key is the string identifier of the template, and the value is the object [`template`](#template)

#### Type template {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | The identifier of the template ||
|| **name**
[`string`](../../data-types.md) | The name of the template ||
|| **region**
[`string`](../../data-types.md) | The region of the template ||
|| **download**
[`string`](../../data-types.md) | The link to download the template ||
|| **users**
[`array`](../../data-types.md) | An array of user or access group codes ||
|| **entityTypeId**
[`array`](../../data-types.md) | An array of bindings to object types ||
|| **downloadMachine**
[`string`](../../data-types.md) | The link for machine downloading of the template ||
|| **code**
[`string`](../../data-types.md) | The symbolic code of the template. Can be `null` ||
|| **active**
[`char`](../../data-types.md) | The activity status (`Y`/`N`) ||
|| **moduleId**
[`string`](../../data-types.md) | The identifier of the template owner module ||
|| **numeratorId**
[`integer`](../../data-types.md) | The identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | The indication of stamp usage (`Y`/`N`) ||
|| **isDeleted**
[`char`](../../data-types.md) | The indication of deletion (`Y`/`N`) ||
|| **sort**
[`integer`](../../data-types.md) | The sorting index ||
|| **createTime**
[`datetime`](../../data-types.md) | The creation time of the template ||
|| **updateTime**
[`datetime`](../../data-types.md) | The last update time of the template ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DOCGEN_ACCESS_ERROR",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to templates ||
|| `Empty value` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-add.md)
- [{#T}](./crm-document-generator-template-update.md)
- [{#T}](./crm-document-generator-template-get.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [Get Document Template Fields crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)