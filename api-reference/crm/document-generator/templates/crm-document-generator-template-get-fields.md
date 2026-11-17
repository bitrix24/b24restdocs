# Get crm.documentgenerator.template.getfields Document Template Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates, permission to modify the specified template, and access to the specified CRM item data

The method `crm.documentgenerator.template.getfields` returns a card of template fields: which fields are available, their current values, default values, and service indicators.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the template ||
|| **entityTypeId**^*^
[`integer`](../../../data-types.md) | Identifier of the CRM object type. Needed to select the data provider ||
|| **entityId**
[`integer`](../../../data-types.md) | Identifier of the CRM object whose data will be used to compute field values. If the parameter is omitted, `null` is used ||
|| **values**
[`object`](../../../data-types.md) | Additional values for computing fields without changing CRM data [(more details)](#parameter-values-fields). The effect of a value depends on the field and provider logic. Defaults to an empty object ||
|#

### Parameter Values {#parameter-values-fields}

The keys in `values` are template field codes, and the values are additional data for computing these fields. The available fields depend on the template and the selected CRM item. For example: `{"DocumentNumber": "2026-001"}`.

The method takes data from `entityTypeId` and `entityId` and computes template fields using `values` and the provider logic. If `values` is omitted, the original CRM data and template logic are used.

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
      title?: string
      value: string | string[] | Array<{ value: string; title: string; selected: boolean }> | null
      default?: string | null
      required?: string
      type?: string
      group?: string[]
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

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.documentgenerator.template.getfields", b24.Params{
    	"id":           1,
    	"entityTypeId": 2,
    	"entityId":     123,
    	"values": b24.Params{
    		"DocumentNumber": "2026-001",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.documentgenerator.template.getfields: %w", err)
    }

    // The method wraps the response in an object with the "templateFields" key.
    raw, ok := b24.Unwrap(res.Result, "templateFields")
    if !ok {
    	return fmt.Errorf("no templateFields key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The response example is abbreviated: the available fields and their number depend on the template.

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
[`object`](../../../data-types.md) | Root element of the response. Contains the [`templateFields`](#result) object ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Type {#result}

#|
|| **Name**
`type` | **Description** ||
|| **templateFields**
[`object`](../../../data-types.md) | Object of template fields, where the key is the field code and the value is the [`templateField`](#templatefield) structure ||
|#

#### templateField Type {#templatefield}

The set of properties depends on the field. For example, `title` and `group` may be absent.

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | Field name ||
|| **value**
```string | array | null``` | Current field value. For a selection field, the array may contain [Value Option Objects](#choice-value) ||
|| **default**
```string | null``` | Default field value ||
|| **required**
[`char`](../../../data-types.md) | Field mandatory indicator: `Y` or `N` ||
|| **type**
[`string`](../../../data-types.md) | Field type, e.g., `IMAGE` ||
|| **group**
[`array`](../../../data-types.md) | Groups to which the field belongs ||
|| **chain**
[`string`](../../../data-types.md) | Path of the field in the data provider, e.g., `this.SOURCE.MY_COMPANY.UF_LOGO` ||
|#

#### Selection Field Value Option {#choice-value}

#|
|| **Name**
`type` | **Description** ||
|| **value**
[`string`](../../../data-types.md) | Option value ||
|| **title**
[`string`](../../../data-types.md) | Option name ||
|| **selected**
[`boolean`](../../../data-types.md) | Whether the option is selected ||
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {entityTypeId} | CRM item type identifier not provided ||
|| `400` | `DOCGEN_ACCESS_ERROR` | Access denied | No access to the CRM item data ||
|| `400` | `0` | Template was not found | Template with the specified `id` not found ||
|| `400` | `100` | Could not find value for parameter {templateId} | Template identifier not provided ||
|| `400` | `0` | Cannot get fields from deleted template | Cannot retrieve fields from a deleted template ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|| `400` | `0` | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|| `400` | `0` | You do not have permissions to modify this template | Insufficient permissions to modify the specified template ||
|| `400` | `0` | No provider for entityTypeId | No data provider found for the specified `entityTypeId` ||
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