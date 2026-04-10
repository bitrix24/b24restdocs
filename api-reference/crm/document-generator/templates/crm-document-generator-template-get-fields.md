# Get Document Template Fields `crm.documentgenerator.template.getfields`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator templates

The method `crm.documentgenerator.template.getfields` returns a card of template fields: which fields are available, their current values, default values, and service indicators.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Template identifier ||
|| **entityTypeId**^*^
[`integer`](../../data-types.md) | Identifier of the CRM entity type. Needed to select the data provider ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the CRM entity whose data will be used to compute field values ||
|| **values**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — field name
- `value_n` — field value

`values` are temporary substitutions over the CRM data. The method takes data by `entityTypeId` and `entityId`, then applies values from `values` and recalculates the template fields.

This allows checking the result without changing the data in CRM. For example, if you pass `values.DocumentNumber = "2026-001"`, the `DocumentNumber` field will return this value in the response.

If `values` is not passed, the method will return the card of fields based on the original CRM data and template logic
||
|#

### Values Parameter {#parameter-values-fields}

The composition of the `values` keys is determined by the template, data provider, and context (`entityTypeId`, `entityId`), so it may vary in different scenarios.

#|
|| **Name**
`type` | **Description** ||
|| **MyCompanyRequisiteRqCompanyName**
[`string`](../../data-types.md) | Short name of the organization ||
|| **MyCompanyRequisiteRegisteredAddressText**
[`string`](../../data-types.md) | Full address ||
|| **MyCompanyPhone**
[`string`](../../data-types.md) | Phone ||
|| **MyCompanyEmail**
[`string`](../../data-types.md) | E-mail ||
|| **MyCompanyWeb**
[`string`](../../data-types.md) | Website ||
|| **MyCompanyUfLogo**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Logo ||
|| **RequisiteRqCompanyName**
[`string`](../../data-types.md) | Short name of the organization ||
|| **RequisiteRegisteredAddressText**
[`string`](../../data-types.md) | Full address ||
|| **ClientPhone**
[`string`](../../data-types.md) | Phone ||
|| **ClientEmail**
[`string`](../../data-types.md) | Email ||
|| **ClientWeb**
[`string`](../../data-types.md) | Website ||
|| **DocumentNumber**
[`string`](../../data-types.md) | Number ||
|| **DocumentCreateTime**
[`string`](../../data-types.md) | Generation date ||
|| **ProductsIndex**
[`string`](../../data-types.md) | Current number ||
|| **ProductsProductName**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Name ||
|| **ProductsProductQuantity**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Quantity ||
|| **ProductsProductMeasureName**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Units of measure ||
|| **ProductsProductPriceRaw**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Original price ||
|| **ProductsProductPriceRawSum**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Total original price ||
|| **TotalRaw**
[`string`](../../data-types.md) | Total original prices ||
|| **TaxesTaxTitle**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Title ||
|| **TaxesTaxRate**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Rate ||
|| **TaxesTaxValue**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Total ||
|| **TotalSum**
[`string`](../../data-types.md) | Total amount ||
|| **MyCompanyAssignedName**
[`string`](../../data-types.md) | First name ||
|| **MyCompanyAssignedLastName**
[`string`](../../data-types.md) | Last name ||
|| **MyCompanyAssignedPersonalPhone**
[`string`](../../data-types.md) | Phone ||
|| **MyCompanyAssignedEmail**
[`string`](../../data-types.md) | E-Mail ||
|| **DocumentTitle**
[`string`](../../data-types.md) | Document title ||
|| **MY_COMPANY**
[`array`](../../data-types.md) | My company ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of retrieving document template fields, where:
- template identifier — `1`
- CRM entity type identifier — `2` (deal)
- CRM entity identifier — `123`
- field value `DocumentNumber` — `2026-001`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"entityTypeId":2,"entityId":123,"values":{"DocumentNumber":"2026-001"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.template.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"entityTypeId":2,"entityId":123,"values":{"DocumentNumber":"2026-001"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.template.getfields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.template.getfields',
    		{
    			id: 1,
    			entityTypeId: 2,
    			entityId: 123,
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
                'crm.documentgenerator.template.getfields',
                [
                    'id' => 1,
                    'entityTypeId' => 2,
                    'entityId' => 123,
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
        echo 'Error getting template fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.template.getfields',
        {
            id: 1,
            entityTypeId: 2,
            entityId: 123,
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
        'crm.documentgenerator.template.getfields',
        [
            'id' => 1,
            'entityTypeId' => 2,
            'entityId' => 123,
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
        "templateFields": {
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
                    "My company"
                ],
                "chain": "this.SOURCE.MY_COMPANY.UF_LOGO",
                "default": null
            },
            "MY_COMPANY": {
                "title": "My company",
                "value": [
                    {
                        "value": "340",
                        "title": "Wheel of Fortune",
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
                    "My company"
                ]
            }
        }
    },
    "time": {
        "start": 1773821944,
        "finish": 1773821944.196063,
        "duration": 0.19606304168701172,
        "processing": 0,
        "date_start": "2026-03-18T11:19:04+02:00",
        "date_finish": "2026-03-18T11:19:04+02:00",
        "operating_reset_at": 1773822544,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the [`templateFields`](#templatefields) object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Type {#result}

#|
|| **Name**
`type` | **Description** ||
|| **templateFields**
[`object`](../../data-types.md) | Object of template fields, where the key is the field code and the value is the [`templateField`](#templatefield) structure ||
|#

#### TemplateField Type {#templatefield}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Field name ||
|| **value**
[`string`](../../data-types.md) \| [`array`](../../data-types.md) | Current field value ||
|| **default**
[`string`](../../data-types.md) | Default field value ||
|| **required**
[`char`](../../data-types.md) | Field mandatory indicator: `Y` or `N` ||
|| **type**
[`string`](../../data-types.md) | Field type, e.g., `IMAGE` ||
|| **group**
[`array`](../../data-types.md) | Groups to which the field belongs ||
|| **chain**
[`string`](../../data-types.md) | Field path in the data provider, e.g., `this.SOURCE.MY_COMPANY.UF_LOGO` ||
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
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the template or insufficient rights to work with document generator templates ||
|| `0` | Template not found | Template with the specified `id` not found or unavailable ||
|| `100` | Bitrix\\DocumentGenerator\\Template constructor must be is public | Required parameter not passed ||
|| `Empty value` | Cannot get fields from deleted template | Cannot get fields from a deleted template ||
|| `Empty value` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-add.md)
- [{#T}](./crm-document-generator-template-update.md)
- [{#T}](./crm-document-generator-template-get.md)
- [{#T}](./crm-document-generator-template-list.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [{#T}](../documents/crm-document-generator-document-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)