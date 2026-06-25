# Get Fields Available for Addition to the Requisite Template crm.requisite.preset.field.availabletoadd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

Returns the fields available for addition to the specified company details template.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template for which to retrieve the list of available customizable fields. 

Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method. 

Fields with the prefix `UF_` in the response are custom fields (see [methods](../../user-fields/index.md) for working with custom requisite fields) ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.availabletoadd
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.availabletoadd
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<string[]>({
        method: 'crm.requisite.preset.field.availabletoadd',
        params: {
          preset: {
            ID: 27,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Available fields:', result, 'Count:', result.length)
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
      async function getAvailablePresetFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.field.availabletoadd',
            params: {
              preset: {
                ID: 27,
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
          console.info('Available fields:', result, 'Count:', result.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAvailablePresetFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.preset.field.availabletoadd',
                [
                    'preset' => [
                        'ID' => 27
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking available fields: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.preset.field.availabletoadd(
            preset={
                "ID": 27,
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
        "crm.requisite.preset.field.availabletoadd",
        {
            preset:
            {
                "ID": 27
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.field.availabletoadd',
        [
            'preset' => ['ID' => 27]
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
    "result": [
        "RQ_FIRST_NAME",
        "RQ_LAST_NAME",
        "RQ_SECOND_NAME",
        "RQ_COMPANY_NAME",
        "RQ_COMPANY_FULL_NAME",
        "RQ_COMPANY_REG_DATE",
        "RQ_DIRECTOR",
        "RQ_ACCOUNTANT",
        "RQ_ADDR",
        "RQ_CONTACT",
        "RQ_EMAIL",
        "RQ_PHONE",
        "RQ_FAX",
        "RQ_IDENT_DOC",
        "RQ_IDENT_DOC_SER",
        "RQ_IDENT_DOC_NUM",
        "RQ_IDENT_DOC_DATE",
        "RQ_IDENT_DOC_ISSUED_BY",
        "RQ_IDENT_DOC_DEP_CODE",
        "RQ_INN",
        "RQ_KPP",
        "RQ_IFNS",
        "RQ_OGRN",
        "RQ_OGRNIP",
        "RQ_OKPO",
        "RQ_OKTMO",
        "RQ_OKVED",
        "RQ_ST_CERT_SER",
        "RQ_ST_CERT_NUM",
        "RQ_ST_CERT_DATE",
        "RQ_SIGNATURE",
        "RQ_STAMP",
        "UF_CRM_1707997209",
        "UF_CRM_1707997236",
        "UF_CRM_1707997253",
        "UF_CRM_1708012333"
    ]
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | An array with the names of fields that can be added to the specified requisite template ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

### Field Descriptions

#|
|| **Name**
`type` | **Description** ||
|| **RQ_FIRST_NAME**
[`string`](../../../../data-types.md) | First Name ||
|| **RQ_LAST_NAME**
[`string`](../../../../data-types.md) | Last Name ||
|| **RQ_SECOND_NAME**
[`string`](../../../../data-types.md) | Patronymic ||
|| **RQ_COMPANY_NAME**
[`string`](../../../../data-types.md) | Short name of the organization ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../../../../data-types.md) | Full name of the organization ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../../../../data-types.md) | Date of state registration ||
|| **RQ_DIRECTOR**
[`string`](../../../../data-types.md) | General director ||
|| **RQ_ACCOUNTANT**
[`string`](../../../../data-types.md) | Chief accountant ||
|| **RQ_CONTACT**
[`string`](../../../../data-types.md) | Contact person ||
|| **RQ_EMAIL**
[`string`](../../../../data-types.md) | E-Mail ||
|| **RQ_PHONE**
[`string`](../../../../data-types.md) | Phone ||
|| **RQ_FAX**
[`string`](../../../../data-types.md) | Fax ||
|| **RQ_IDENT_DOC**
[`string`](../../../../data-types.md) | Type of document ||
|| **RQ_IDENT_DOC_SER**
[`string`](../../../../data-types.md) | Series ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../../../../data-types.md) | Number ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../../../../data-types.md) | Date of issue ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../../../../data-types.md) | Issued by ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../../../../data-types.md) | Department code ||
|| **RQ_INN**
[`string`](../../../../data-types.md) | TIN ||
|| **RQ_KPP**
[`string`](../../../../data-types.md) | KPP ||
|| **RQ_IFNS**
[`string`](../../../../data-types.md) | IFNS ||
|| **RQ_OGRN**
[`string`](../../../../data-types.md) | OGRN ||
|| **RQ_OGRNIP**
[`string`](../../../../data-types.md) | OGRNIP ||
|| **RQ_OKPO**
[`string`](../../../../data-types.md) | OKPO ||
|| **RQ_OKTMO**
[`string`](../../../../data-types.md) | OKTMO ||
|| **RQ_OKVED**
[`string`](../../../../data-types.md) | OKVED ||
|| **RQ_ST_CERT_SER**
[`string`](../../../../data-types.md) | Series of State Registration Certificate ||
|| **RQ_ST_CERT_NUM**
[`string`](../../../../data-types.md) | Number of State Registration Certificate ||
|| **RQ_ST_CERT_DATE**
[`string`](../../../../data-types.md) | Date of State Registration Certificate ||
|| **UF_CRM_1707997209**
[`double`](../../../../data-types.md) | Custom Field of Type "Number" ||
|| **UF_CRM_1707997236**
[`boolean`](../../../../data-types.md) | Custom Field of Type "Yes/No" ||
|| **UF_CRM_1707997253**
[`datetime`](../../../../data-types.md) | Custom Field of Type "Date" ||
|| **UF_CRM_1708012333**
[`string`](../../../../data-types.md) | Custom Field of Type "String" ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Template not found."
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| Template not found | The template for which to retrieve the list of available fields was not found ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)