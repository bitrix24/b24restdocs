# Get the list of document templates crm.documentgenerator.template.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for document generator templates

The method `crm.documentgenerator.template.list` returns a list of document templates.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of fields to return for the templates.

You can use:
- `'*'` — to select all standard fields of the template
- an explicit list of fields, for example `["id","name","region","active"]`

Additionally, the following are supported:
- `entityTypeId` — array of template bindings to CRM entities
- `users` — array of access permission codes

Main fields for `select`: `id`, `name`, `region`, `code`, `active`, `moduleId`, `numeratorId`, `withStamps`, `isDeleted`, `sort`, `createTime`, `updateTime`

See the list of template fields in the section [`Type template`](#template). By default, `["*"]` is used. ||
|| **filter**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — name of the field for filtering
- `value_n` — filter value

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
- the filter `moduleId` is forcibly limited to the value `crm`
- you can filter by `entityTypeId`, for example `["2","2_category_37"]`

Main fields for `filter`: `id`, `name`, `region`, `code`, `active`, `moduleId`, `numeratorId`, `withStamps`, `isDeleted`, `sort`, `createTime`, `updateTime`, `entityTypeId` ||
|| **order**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — name of the sorting field
- `value_n` — sorting direction: `ASC` or `DESC`

Main fields for `order`: `id`, `name`, `region`, `code`, `active`, `moduleId`, `numeratorId`, `withStamps`, `isDeleted`, `sort`, `createTime`, `updateTime`

Example: `{"id":"DESC","sort":"ASC"}` ||
|| **start**
[`integer`](../../data-types.md) | Pagination parameter.

The page size is fixed: `50` records.

Formula for obtaining the N-th page:
`start = (N - 1) * 50`

More details in the article [Features of list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of obtaining a list of templates where:
- fields `id`, `name`, `region`, `entityTypeId`, `users` are selected
- sorting by `id` in descending order
- filter by region `de` and activity `Y`
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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.template.list',
    		{
    			select: ['id', 'name', 'region', 'entityTypeId', 'users'],
    			order: { id: 'desc' },
    			filter: { region: 'de', active: 'Y' },
    			start: 0,
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error('Error:', error);
    }
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
            start: 0,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
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
            'filter' => ['region': 'de', 'active': 'Y'],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "templates": {
            "39": {
                "id": "39",
                "name": "Demonstration of product implementation",
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
                "name": "Act of write-off of products (Germany)",
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
        "date_start": "2026-03-18T17:51:19+02:00",
        "date_finish": "2026-03-18T17:51:19+02:00",
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
[`object`](../../data-types.md) | Root element of the response. Contains the object [`templates`](#templates) ||
|| **total**
[`integer`](../../data-types.md) | Total number of templates matching the filter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object templates {#templates}

[`object`](../../data-types.md), where the key is the string identifier of the template, and the value is the object [`template`](#template)

#### Type template {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the template ||
|| **name**
[`string`](../../data-types.md) | Name of the template ||
|| **region**
[`string`](../../data-types.md) | Region of the template ||
|| **download**
[`string`](../../data-types.md) | Link to download the template ||
|| **users**
[`array`](../../data-types.md) | Array of user or access group codes ||
|| **entityTypeId**
[`array`](../../data-types.md) | Array of bindings to object types ||
|| **downloadMachine**
[`string`](../../data-types.md) | Link for machine download of the template ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the template. Can be `null` ||
|| **active**
[`char`](../../data-types.md) | Activity status (`Y`/`N`) ||
|| **moduleId**
[`string`](../../data-types.md) | Identifier of the module owning the template ||
|| **numeratorId**
[`integer`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Indicator of stamp usage (`Y`/`N`) ||
|| **isDeleted**
[`char`](../../data-types.md) | Indicator of deletion (`Y`/`N`) ||
|| **sort**
[`integer`](../../data-types.md) | Sorting index ||
|| **createTime**
[`datetime`](../../data-types.md) | Time of template creation ||
|| **updateTime**
[`datetime`](../../data-types.md) | Time of last template update ||
|#

## Error Handling

HTTP status: **400**

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
- [Get the fields of the document template crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)