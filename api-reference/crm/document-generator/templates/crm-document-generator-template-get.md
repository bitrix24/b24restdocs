# Get Information About Document Template by Id crm.documentgenerator.template.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for document generator templates

The method `crm.documentgenerator.template.get` returns information about a template by its identifier.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Template identifier ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

Example of retrieving a document template, where:
- template identifier — `41`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":41}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.template.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":41,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.template.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.template.get',
    		{
    			id: 41,
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
                'crm.documentgenerator.template.get',
                [
                    'id' => 41,
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
        echo 'Error getting template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.template.get',
        {
            id: 41,
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
        'crm.documentgenerator.template.get',
        [
            'id' => 41,
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
        "template": {
            "id": "41",
            "name": "Template from file (updated)",
            "region": "de",
            "code": null,
            "download": "https://mysite.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=41",
            "downloadMachine": "https://mysite.com/rest/crm.documentgenerator.template.download.json?auth=***&token=***",
            "active": "Y",
            "moduleId": "crm",
            "numeratorId": "49",
            "withStamps": "N",
            "users": {
                "UA": "UA"
            },
            "isDeleted": "N",
            "sort": "500",
            "entityTypeId": [
                "2"
            ],
            "createTime": "2026-03-18T15:54:20+02:00",
            "updateTime": "2026-03-18T17:26:29+02:00"
        }
    },
    "time": {
        "start": 1773846100,
        "finish": 1773846100.102531,
        "duration": 0.10253095626831055,
        "processing": 0,
        "date_start": "2026-03-18T18:01:40+02:00",
        "date_finish": "2026-03-18T18:01:40+02:00",
        "operating_reset_at": 1773846700,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the object [`template`](#template) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type template {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Template identifier ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **code**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Symbolic code of the template ||
|| **download**
[`string`](../../data-types.md) | Link to download the template file ||
|| **downloadMachine**
[`string`](../../data-types.md) | Link to download the template file for the application ||
|| **active**
[`char`](../../data-types.md) | Template activity: `Y` or `N` ||
|| **moduleId**
[`string`](../../data-types.md) | Module identifier ||
|| **numeratorId**
[`string`](../../data-types.md) | Numerator identifier ||
|| **withStamps**
[`char`](../../data-types.md) | Include stamp and signature: `Y` or `N` ||
|| **users**
[`object`](../../data-types.md) | Object of access permission codes in the format `{"UA":"UA"}` ||
|| **isDeleted**
[`char`](../../data-types.md) | Template deletion status: `Y` or `N` ||
|| **sort**
[`string`](../../data-types.md) | Sort index ||
|| **entityTypeId**
[`array`](../../data-types.md) | Array of CRM entity identifiers for which the template is available ||
|| **createTime**
[`datetime`](../../data-types.md) | Date and time of template creation ||
|| **updateTime**
[`datetime`](../../data-types.md) | Date and time of the last template update ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Template not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Template not found | Template with the specified `id` not found or unavailable ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the template ||
|| `Empty value` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-add.md)
- [{#T}](./crm-document-generator-template-update.md)
- [{#T}](./crm-document-generator-template-list.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [Get Document Template Fields crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)