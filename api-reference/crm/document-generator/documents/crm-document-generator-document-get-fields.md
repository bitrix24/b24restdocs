# Get Document Fields crm.documentgenerator.document.getfields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator documents

The method `crm.documentgenerator.document.getfields` returns a detail form of the fields of an already created document: which fields are available, their current values, default values, and service indicators.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Document identifier ||
|| **values**
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
- `field_n` — document field code
- `value_n` — field value

`values` are temporary substitutions over the current document values. The method takes the document data, applies `values`, and recalculates the fields.

This allows checking the result without changing the document itself. For example, if you pass `values.DocumentNumber = "2026-001"`, the response will return the `DocumentNumber` field with the value `2026-001`. If `values` are not passed, the current value of the document will be returned for the same field, for instance, `1`.

If `values` are not provided, the method will return the detail form of the fields based on the current document data and template logic ||
|#

### Parameter values {#parameter-values-fields}

The composition of the keys in `values` depends on the template used to create the document, so it may vary across different documents.

#|
|| **Name**
`type` | **Description** ||
|| **DocumentNumber**
[`string`](../../data-types.md) | Document number ||
|| **DocumentCreateTime**
[`string`](../../data-types.md) | Generation date ||
|| **DocumentTitle**
[`string`](../../data-types.md) | Document title ||
|| **ClientPhone**
[`string`](../../data-types.md) | Client phone ||
|| **ClientEmail**
[`string`](../../data-types.md) | Client email ||
|| **ProductsProductName**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Product name ||
|| **ProductsProductQuantity**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Quantity ||
|| **TotalSum**
[`string`](../../data-types.md) | Total amount ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of retrieving document fields, where:
- document identifier — `101`
- field value `DocumentNumber` — `2026-001`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":101,"values":{"DocumentNumber":"2026-001"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":101,"values":{"DocumentNumber":"2026-001"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.getfields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.document.getfields',
    		{
    			id: 101,
    			values: {
    				DocumentNumber: '2026-001',
    			},
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
                'crm.documentgenerator.document.getfields',
                [
                    'id' => 101,
                    'values' => [
                        'DocumentNumber' => '2026-001',
                    ],
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
        echo 'Error getting document fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.getfields',
        {
            id: 101,
            values: {
                DocumentNumber: '2026-001',
            },
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
        'crm.documentgenerator.document.getfields',
        [
            'id' => 101,
            'values' => [
                'DocumentNumber' => '2026-001',
            ],
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
        "documentFields": {
            "DocumentNumber": {
                "title": "Number",
                "value": "2026-001",
                "required": "Y",
                "group": [
                    "Document"
                ],
                "chain": "this.DOCUMENT.DOCUMENT_NUMBER",
                "default": "2026-001"
            },
            "MyCompanyUfLogo": {
                "title": "Logo",
                "value": null,
                "type": "IMAGE",
                "group": [
                    "Document",
                    "My Company"
                ],
                "chain": "this.SOURCE.MY_COMPANY.UF_LOGO",
                "default": null
            },
            "MY_COMPANY": {
                "title": "My Company",
                "value": [
                    {
                        "value": "340",
                        "title": "Fortune Wheel",
                        "selected": true
                    },
                    {
                        "value": "358",
                        "title": "Bitrix-Development",
                        "selected": false
                    }
                ],
                "group": [
                    "Document",
                    "My Company"
                ]
            }
        }
    },
    "time": {
        "start": 1773909500,
        "finish": 1773909500.192341,
        "duration": 0.19234108924865723,
        "processing": 0,
        "date_start": "2026-03-19T11:38:20+02:00",
        "date_finish": "2026-03-19T11:38:20+02:00",
        "operating_reset_at": 1773910100,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the object [`result`](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **documentFields**
[`object`](../../data-types.md) | Object of document fields, where the key is the field code and the value is the structure [`documentField`](#documentfield) ||
|#

#### Type documentField {#documentfield}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Field name ||
|| **value**
[`string`](../../data-types.md) \| [`array`](../../data-types.md) \| [`null`](../../data-types.md) | Current field value ||
|| **default**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Default field value ||
|| **required**
[`char`](../../data-types.md) | Field mandatory indicator: `Y` or `N` ||
|| **type**
[`string`](../../data-types.md) | Field type, e.g., `IMAGE` ||
|| **group**
[`array`](../../data-types.md) | Groups to which the field belongs ||
|| **chain**
[`string`](../../data-types.md) \| [`array`](../../data-types.md) | Path of the field in the data provider, e.g., `this.SOURCE.MY_COMPANY.UF_LOGO` ||
|| **format**
[`object`](../../data-types.md) | Field formatting parameters, e.g., `{"currencyId":"EUR","withZeros":true}` ||
|| **options**
[`object`](../../data-types.md) | Additional field parameters, e.g., `{"isArray":true}` ||
|| **hideRow**
[`char`](../../data-types.md) | Service indicator for hiding the row: `Y` or `N` ||
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
|| `100` | Bitrix\\DocumentGenerator\\Document constructor must be is public | Required parameter `id` not provided ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the document or insufficient rights to work with document generator documents ||
|| `0` | Document not found | Document with the specified `id` not found or unavailable ||
|| `Empty value` | You do not have permissions to modify documents | Insufficient rights to modify document generator documents ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-list.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](../templates/crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)