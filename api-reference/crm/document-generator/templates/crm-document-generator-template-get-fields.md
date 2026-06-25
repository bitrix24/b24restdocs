# Get crm.documentgenerator.template.getfields Document Template Fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for Document Generator templates

The method `crm.documentgenerator.template.getfields` returns a card of template fields: which fields are available, their current values, default values, and service indicators.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Identifier of the template ||
|| **entityTypeId**^*^
[`integer`](../../data-types.md) | Identifier of the CRM object type. Needed to select the data provider ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the CRM object whose data will be used to compute field values ||
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

### Parameter Values {#parameter-values-fields}

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
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Heading ||
|| **TaxesTaxRate**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Rate ||
|| **TaxesTaxValue**
[`array`](../../data-types.md) \| [`string`](../../data-types.md) | Amount ||
|| **TotalSum**
[`string`](../../data-types.md) | Total amount ||
|| **MyCompanyAssignedName**
[`string`](../../data-types.md) | First Name ||
|| **MyCompanyAssignedLastName**
[`string`](../../data-types.md) | Last Name ||
|| **MyCompanyAssignedPersonalPhone**
[`string`](../../data-types.md) | Phone ||
|| **MyCompanyAssignedEmail**
[`string`](../../data-types.md) | E-Mail ||
|| **DocumentTitle**
[`string`](../../data-types.md) | Document name ||
|| **MY_COMPANY**
[`array`](../../data-types.md) | My company ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of retrieving document template fields, where:
- template identifier — `1`
- CRM object type identifier — `2` (deal)
- CRM object identifier — `123`
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetFieldsResult = {
      templateFields: Record<string, TemplateField>
    }

    type TemplateField = {
      title: string
      value: string | string[] | null
      default?: string | null
      required?: string
      type?: string
      group: string[]
      chain?: string
    }

    try {
      const response = await $b24.actions.v2.call.make<GetFieldsResult>({
        method: 'crm.documentgenerator.template.getfields',
        params: {
          id: 1,
          entityTypeId: 2,
          entityId: 123,
          values: {
            DocumentNumber: '2026-001',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Template fields:', Object.keys(result.templateFields))
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
      async function getTemplateFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.template.getfields',
            params: {
              id: 1,
              entityTypeId: 2,
              entityId: 123,
              values: {
                DocumentNumber: '2026-001',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Template fields:', Object.keys(result.templateFields))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTemplateFields)
    </script>
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

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.template.getfields(
            bitrix_id=1,
            entity_type_id=2,
            entity_id=123,
            values={
                "DocumentNumber": "2026-001",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

HTTP status: **200**

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
                        "title": "Wheel of Fortune",
                        "selected": true
                    },
                    {
                        "value": "358",
                        "title": "Bitrix Development",
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
        "start": 1773821944,
        "finish": 1773821944.196063,
        "duration": 0.19606304168701172,
        "processing": 0,
        "date_start": "2026-03-18T11:19:04+03:00",
        "date_finish": "2026-03-18T11:19:04+03:00",
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
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Type {#result}

#|
|| **Name**
`type` | **Description** ||
|| **templateFields**
[`object`](../../data-types.md) | Object of template fields, where the key is the field code and the value is the [`templateField`](#templatefield) structure ||
|#

#### templateField Type {#templatefield}

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
[`string`](../../data-types.md) | Path of the field in the data provider, e.g., `this.SOURCE.MY_COMPANY.UF_LOGO` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "DOCGEN_ACCESS_ERROR",
    "error_description": "Access denied"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the template or insufficient rights to work with document generator templates ||
|| `0` | Template not found | Template with the specified `id` not found or unavailable ||
|| `100` | Bitrix\\DocumentGenerator\\Template constructor must be is public | A required parameter was not provided ||
|| Empty value | Cannot get fields from deleted template | Cannot get fields from a deleted template ||
|| Empty value | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|| Empty value | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-add.md)
- [{#T}](./crm-document-generator-template-update.md)
- [{#T}](./crm-document-generator-template-get.md)
- [{#T}](./crm-document-generator-template-list.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [{#T}](../documents/crm-document-generator-document-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)