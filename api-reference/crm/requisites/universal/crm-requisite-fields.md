# Get CRM Requisite Fields crm.requisite.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `crm.requisite.fields` method returns descriptions of system and custom detail fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldInfo = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
      statusType?: string[]
      listLabel?: string
      formLabel?: string
      filterLabel?: string
      settings?: Record<string, unknown>
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RequisiteFieldsResult = Record<string, FieldInfo>

    try {
      const response = await $b24.actions.v2.call.make<RequisiteFieldsResult>({
        method: 'crm.requisite.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result).length, result)
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
      async function getRequisiteFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result).length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getRequisiteFields)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.requisite.fields().response
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
                'crm.requisite.fields',
                []
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
        echo 'Error fetching requisite fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "crm.requisite.fields",
        {},
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
        'crm.requisite.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.requisite.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.requisite.fields: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "ENTITY_TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Entity type ID"
        },
        "ENTITY_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Entity ID"
        },
        "PRESET_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Preset ID"
        },
        "DATE_CREATE": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Creation date"
        },
        "DATE_MODIFY": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Modification date"
        },
        "CREATED_BY_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Created by"
        },
        "MODIFY_BY_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Modified by"
        },
        "NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Name"
        },
        "CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Code"
        },
        "XML_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External code"
        },
        "ORIGINATOR_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ORIGINATOR_ID"
        },
        "ACTIVE": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Active"
        },
        "ADDRESS_ONLY": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ADDRESS_ONLY"
        },
        "SORT": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Sort order"
        },
        "RQ_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Full Name"
        },
        "RQ_FIRST_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "First Name"
        },
        "RQ_LAST_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Last Name"
        },
        "RQ_SECOND_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Middle Name"
        },
        "RQ_COMPANY_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_COMPANY_ID"
        },
        "RQ_COMPANY_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Short organization name"
        },
        "RQ_COMPANY_FULL_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Full organization name"
        },
        "RQ_COMPANY_REG_DATE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "State registration date"
        },
        "RQ_DIRECTOR": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "CEO"
        },
        "RQ_ACCOUNTANT": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Chief Accountant"
        },
        "RQ_CEO_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CEO_NAME"
        },
        "RQ_CEO_WORK_POS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CEO_WORK_POS"
        },
        "RQ_CONTACT": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Contact person"
        },
        "RQ_EMAIL": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "E-Mail"
        },
        "RQ_PHONE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Phone"
        },
        "RQ_FAX": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Fax"
        },
        "RQ_IDENT_TYPE": {
            "type": "crm_status",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "statusType": [
                "RQ_IDENT_TYPE_CO"
            ],
            "title": "RQ_IDENT_TYPE"
        },
        "RQ_IDENT_DOC": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Document type"
        },
        "RQ_IDENT_DOC_SER": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "series"
        },
        "RQ_IDENT_DOC_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "number"
        },
        "RQ_IDENT_DOC_PERS_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_IDENT_DOC_PERS_NUM"
        },
        "RQ_IDENT_DOC_DATE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "issue date"
        },
        "RQ_IDENT_DOC_ISSUED_BY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "issued by"
        },
        "RQ_IDENT_DOC_DEP_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "department code"
        },
        "RQ_INN": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Tax ID"
        },
        "RQ_KPP": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Tax Registration Code"
        },
        "RQ_USRLE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_USRLE"
        },
        "RQ_IFNS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Tax Office"
        },
        "RQ_OGRN": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "State Registration Number"
        },
        "RQ_OGRNIP": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Sole Proprietor Registration Number"
        },
        "RQ_OKPO": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "OKPO Code"
        },
        "RQ_OKTMO": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "OKTMO Code"
        },
        "RQ_OKVED": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "NAICS Code"
        },
        "RQ_EDRPOU": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_EDRPOU"
        },
        "RQ_DRFO": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_DRFO"
        },
        "RQ_KBE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_KBE"
        },
        "RQ_IIN": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_IIN"
        },
        "RQ_BIN": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_BIN"
        },
        "RQ_ST_CERT_SER": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "State registration certificate series"
        },
        "RQ_ST_CERT_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "State registration certificate number"
        },
        "RQ_ST_CERT_DATE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "State registration certificate date"
        },
        "RQ_VAT_PAYER": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_VAT_PAYER"
        },
        "RQ_VAT_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_VAT_ID"
        },
        "RQ_VAT_CERT_SER": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_VAT_CERT_SER"
        },
        "RQ_VAT_CERT_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_VAT_CERT_NUM"
        },
        "RQ_VAT_CERT_DATE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_VAT_CERT_DATE"
        },
        "RQ_RESIDENCE_COUNTRY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_RESIDENCE_COUNTRY"
        },
        "RQ_BASE_DOC": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_BASE_DOC"
        },
        "RQ_REGON": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_REGON"
        },
        "RQ_KRS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_KRS"
        },
        "RQ_PESEL": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_PESEL"
        },
        "RQ_LEGAL_FORM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_LEGAL_FORM"
        },
        "RQ_SIRET": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_SIRET"
        },
        "RQ_SIREN": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_SIREN"
        },
        "RQ_CAPITAL": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CAPITAL"
        },
        "RQ_RCS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_RCS"
        },
        "RQ_CNPJ": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CNPJ"
        },
        "RQ_STATE_REG": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_STATE_REG"
        },
        "RQ_MNPL_REG": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_MNPL_REG"
        },
        "RQ_CPF": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CPF"
        },
        "RQ_TAX_REGIME": {
            "type": "crm_status",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "statusType": [
                "RQ_TAX_REGIME_MX"
            ],
            "title": "RQ_TAX_REGIME"
        },
        "UF_CRM_1694526604": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": true,
            "title": "UF_CRM_1694526604",
            "listLabel": "PP - String",
            "formLabel": "PP - String",
            "filterLabel": "PP - String",
            "settings": {
                "SIZE": 20,
                "ROWS": 1,
                "REGEXP": "",
                "MIN_LENGTH": 0,
                "MAX_LENGTH": 0,
                "DEFAULT_VALUE": null
            }
        },
        "UF_CRM_1707997209": {
            "type": "double",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": true,
            "title": "UF_CRM_1707997209",
            "listLabel": "PP - Number",
            "formLabel": "PP - Number",
            "filterLabel": "PP - Number",
            "settings": {
                "PRECISION": 2,
                "SIZE": 20,
                "MIN_VALUE": 0,
                "MAX_VALUE": 0,
                "DEFAULT_VALUE": null
            }
        },
        "UF_CRM_1707997236": {
            "type": "boolean",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": true,
            "title": "UF_CRM_1707997236",
            "listLabel": "PP - Yes/No",
            "formLabel": "PP - Yes/No",
            "filterLabel": "PP - Yes/No",
            "settings": {
                "DEFAULT_VALUE": 0,
                "DISPLAY": "CHECKBOX",
                "LABEL": [
                    "",
                    ""
                ],
                "LABEL_CHECKBOX": "PP - Yes/No"
            }
        },
        "UF_CRM_1707997253": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": true,
            "title": "UF_CRM_1707997253",
            "listLabel": "PP - Date",
            "formLabel": "PP - Date",
            "filterLabel": "PP - Date",
            "settings": {
                "DEFAULT_VALUE": {
                    "TYPE": "NONE",
                    "VALUE": ""
                },
                "USE_SECOND": "Y",
                "USE_TIMEZONE": "N"
            }
        }
    },
    "time": {
        "start": 1716902185.003805,
        "finish": 1716902185.379388,
        "duration": 0.3755831718444824,
        "processing": 0.016958951950073242,
        "date_start": "2024-05-28T15:16:25+02:00",
        "date_finish": "2024-05-28T15:16:25+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the [detail field](#result-fields), and `value` is an object with [field attributes](#attributes) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#


#### Detail Fields {#result-fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Details identifier. Created automatically ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Parent object type identifier.

Currently, this can only be:
- `3` — contact
- `4` — company

The [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method returns identifiers for all CRM object types

||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Parent object identifier (contact or company).

The identifier can be obtained using the [crm.company.list](../../companies/crm-company-list.md) method for a company and the [crm.contact.list](../../contacts/crm-contact-list.md) method for a contact ||
|| **PRESET_ID**
[`integer`](../../../data-types.md) | Identifier of the requisite template.

Template identifiers can be obtained using the method [crm.requisite.preset.list](../presets/crm-requisite-preset-list.md) ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who changed the requisite ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the requisite ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key, used for exchange operations.

Identifier of the external information database.

The purpose of the field may change by the final developer ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information database.

The purpose of the field may change by the final developer ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status.

Values `Y` or `N` are used.

The default value is `Y`. Currently, the field does not affect anything ||
|| **ADDRESS_ONLY**
[`char`](../../../data-types.md) | A status flag indicating when the requisite is used only for storing an address.

Values `Y` or `N` are used. The default value is `N`.

When set to `Y`, the details are not displayed in the object form, but the address is shown ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting.

Order in the object's details list when there are multiple entries. The default value is `500` ||
|| **RQ_NAME**
[`string`](../../../data-types.md) | Full Name ||
|| **RQ_FIRST_NAME**
[`string`](../../../data-types.md) | First Name ||
|| **RQ_LAST_NAME**
[`string`](../../../data-types.md) | Last Name ||
|| **RQ_SECOND_NAME**
[`string`](../../../data-types.md) | Middle Name ||
|| **RQ_COMPANY_ID**
[`string`](../../../data-types.md) | Identifier of the organization ||
|| **RQ_COMPANY_NAME**
[`string`](../../../data-types.md) | Short name of the organization ||
|| **RQ_COMPANY_FULL_NAME**
[`string`](../../../data-types.md) | Full name of the organization ||
|| **RQ_COMPANY_REG_DATE**
[`string`](../../../data-types.md) | State registration date in string format, for example `06.04.2007` ||
|| **RQ_DIRECTOR**
[`string`](../../../data-types.md) | General director ||
|| **RQ_ACCOUNTANT**
[`string`](../../../data-types.md) | Chief accountant ||
|| **RQ_CEO_NAME**
[`string`](../../../data-types.md) | Full name of the first leader ||
|| **RQ_CEO_WORK_POS**
[`string`](../../../data-types.md) | Position of the first leader ||
|| **RQ_CONTACT**
[`string`](../../../data-types.md) | Contact person ||
|| **RQ_EMAIL**
[`string`](../../../data-types.md) | E-Mail ||
|| **RQ_PHONE**
[`string`](../../../data-types.md) | Phone ||
|| **RQ_FAX**
[`string`](../../../data-types.md) | Fax ||
|| **RQ_IDENT_TYPE**
[`crm_status`](../../data-types.md) | Identification method. Directory identifiers for available countries are in the `statusType` attribute. To get directory values, pass its identifier in `filter[ENTITY_ID]` of the [crm.status.list](../../status/crm-status-list.md) method ||
|| **RQ_IDENT_DOC**
[`string`](../../../data-types.md) | Type of document ||
|| **RQ_IDENT_DOC_SER**
[`string`](../../../data-types.md) | Series ||
|| **RQ_IDENT_DOC_NUM**
[`string`](../../../data-types.md) | Number ||
|| **RQ_IDENT_DOC_PERS_NUM**
[`string`](../../../data-types.md) | Personal number ||
|| **RQ_IDENT_DOC_DATE**
[`string`](../../../data-types.md) | Issue date in string format, for example `21.02.2015` ||
|| **RQ_IDENT_DOC_ISSUED_BY**
[`string`](../../../data-types.md) | Issued by ||
|| **RQ_IDENT_DOC_DEP_CODE**
[`string`](../../../data-types.md) | Department code ||
|| **RQ_INN**
[`string`](../../../data-types.md) | TIN ||
|| **RQ_KPP**
[`string`](../../../data-types.md) | KPP ||
|| **RQ_USRLE**
[`string`](../../../data-types.md) | Handelsregisternummer (for country DE) ||
|| **RQ_IFNS**
[`string`](../../../data-types.md) | IFNS ||
|| **RQ_OGRN**
[`string`](../../../data-types.md) | OGRN ||
|| **RQ_OGRNIP**
[`string`](../../../data-types.md) | OGRNIP ||
|| **RQ_OKPO**
[`string`](../../../data-types.md) | OKPO ||
|| **RQ_OKTMO**
[`string`](../../../data-types.md) | OKTMO ||
|| **RQ_OKVED**
[`string`](../../../data-types.md) | OKVED ||
|| **RQ_EDRPOU**
[`string`](../../../data-types.md) | EDRPOU ||
|| **RQ_DRFO**
[`string`](../../../data-types.md) | DRFO ||
|| **RQ_KBE**
[`string`](../../../data-types.md) | KBE ||
|| **RQ_IIN**
[`string`](../../../data-types.md) | IIN ||
|| **RQ_BIN**
[`string`](../../../data-types.md) | BIN ||
|| **RQ_ST_CERT_SER**
[`string`](../../../data-types.md) | Series of State Registration Certificate ||
|| **RQ_ST_CERT_NUM**
[`string`](../../../data-types.md) | Number of State Registration Certificate ||
|| **RQ_ST_CERT_DATE**
[`string`](../../../data-types.md) | Date of State Registration Certificate ||
|| **RQ_VAT_PAYER**
[`char`](../../../data-types.md) | VAT payer (for country UA).

Values `Y` or `N` are used. The default value is `N` ||
|| **RQ_VAT_ID**
[`string`](../../../data-types.md) | VAT ID (identification number of VAT payer) ||
|| **RQ_VAT_CERT_SER**
[`string`](../../../data-types.md) | Series of the VAT certificate ||
|| **RQ_VAT_CERT_NUM**
[`string`](../../../data-types.md) | Number of the VAT certificate ||
|| **RQ_VAT_CERT_DATE**
[`string`](../../../data-types.md) | Date of the VAT certificate ||
|| **RQ_RESIDENCE_COUNTRY**
[`string`](../../../data-types.md) | Country of residence ||
|| **RQ_BASE_DOC**
[`string`](../../../data-types.md) | Basis for action ||
|| **RQ_REGON**
[`string`](../../../data-types.md) | REGON (for country PL) ||
|| **RQ_KRS**
[`string`](../../../data-types.md) | KRS (for country PL) ||
|| **RQ_PESEL**
[`string`](../../../data-types.md) | PESEL (for country PL) ||
|| **RQ_LEGAL_FORM**
[`string`](../../../data-types.md) | Legal form (for country FR) ||
|| **RQ_SIRET**
[`string`](../../../data-types.md) | Siret number (for country FR) ||
|| **RQ_SIREN**
[`string`](../../../data-types.md) | Siren number (for country FR) ||
|| **RQ_CAPITAL**
[`string`](../../../data-types.md) | Share capital (for country FR) ||
|| **RQ_RCS**
[`string`](../../../data-types.md) | RCS (for country FR) ||
|| **RQ_CNPJ**
[`string`](../../../data-types.md) | CNPJ (for country BR) ||
|| **RQ_STATE_REG**
[`string`](../../../data-types.md) | State Registration (IE) (for country BR) ||
|| **RQ_MNPL_REG**
[`string`](../../../data-types.md) | Municipal Registration (IM) (for country BR) ||
|| **RQ_CPF**
[`string`](../../../data-types.md) | CPF (for country BR) ||
|| **RQ_TAX_REGIME**
[`crm_status`](../../data-types.md) | Tax regime for country MX. The directory identifier is in the `statusType` attribute. To get directory values, pass its identifier in `filter[ENTITY_ID]` of the [crm.status.list](../../status/crm-status-list.md) method ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_1694526604`.

Requisites can have a set of custom fields with types: `string`, `boolean`, `double`, `datetime`.

You can add a custom field to requisites using the method [crm.requisite.userfield.add](../user-fields/crm-requisite-userfield-add.md) ||
|#

#### Attribute Description {#attributes}

#|
|| **Name**
`type` | **Description** ||
|| type
[`string`](../../../data-types.md) | Field type ||
|| statusType
[`array`](../../../data-types.md) | Directory identifiers for a `crm_status` field. To get directory values, pass its identifier in `filter[ENTITY_ID]` of the [crm.status.list](../../status/crm-status-list.md) method ||
|| isRequired
[`boolean`](../../../data-types.md) | Required attribute
- `true` — yes
- `false` — no

||
|| isReadOnly
[`boolean`](../../../data-types.md) | Read-only attribute
- `true` — yes
- `false` — no

||
|| isImmutable
[`boolean`](../../../data-types.md) | Immutable attribute
- `true` — yes
- `false` — no

||
|| isMultiple
[`boolean`](../../../data-types.md) | Multi-field attribute
- `true` — yes
- `false` — no

||
|| isDynamic
[`boolean`](../../../data-types.md) | Custom attribute
- `true` — yes
- `false` — no

||
|| title
[`string`](../../../data-types.md) | Field identifier ||
|| listLabel
[`string`](../../../data-types.md) | Custom field attribute. Contains the field name in lists ||
|| formLabel
[`string`](../../../data-types.md) | Custom field attribute. Contains the field name in the detail form ||
|| filterLabel
[`string`](../../../data-types.md) | Custom field attribute. Contains the field name in the filter ||
|| settings
[`object`](../../../data-types.md) | Custom field attribute. An object with specific settings for a particular field type. See [custom requisite fields](../user-fields/index.md) ||
|#

## Error Handling

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-add.md)
- [{#T}](./crm-requisite-update.md)
- [{#T}](./crm-requisite-get.md)
- [{#T}](./crm-requisite-list.md)
- [{#T}](./crm-requisite-delete.md)


