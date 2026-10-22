# Get the CRM Form Builder Configuration crm.form.configuration.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.form.configuration.get` method returns the Bitrix24 directories and settings required to build a CRM form.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.configuration.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.configuration.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.configuration.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type NamedId = { id: number, name: string }
    type NamedCode = { id: string, name: string }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormConfigurationResult = {
      item: {
        entitySchemes: Array<{
          id: number
          name: string
          description: string
          entityNames: string[]
          mainEntityName: string | null
          hasInvoice: boolean
          dynamic: boolean
        }>
        duplicateModes: NamedCode[]
        lead: { enabled: boolean }
        dealCategories: NamedId[]
        smartProcesses: Array<{
          entityTypeId: number
          entityName: string
          name: string
          categories: NamedId[]
        }>
        languages: NamedCode[]
        currencies: Array<{ id: string, fullName: string, formatString: string }>
        fieldTypes: Array<{ id: string, name: string, parameters: string[] }>
        paySystems: NamedId[]
        permissions: { canRead: boolean, canEdit: boolean }
        limits: { activeForms: { current: number, maximum: number } | null }
        product: { isCloud: boolean }
        region: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FormConfigurationResult>({
        method: 'crm.form.configuration.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.entitySchemes)
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
      async function getFormConfiguration() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.configuration.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.entitySchemes)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFormConfiguration)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.form.configuration.get().response
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.configuration.get',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.configuration.get',
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.form.configuration.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.form.configuration.get", b24.Params{}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.configuration.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	EntitySchemes []struct {
    		ID          int      `json:"id"`
    		Name        string   `json:"name"`
    		EntityNames []string `json:"entityNames"`
    	} `json:"entitySchemes"`
    	Limits struct {
    		ActiveForms *struct {
    			Current int `json:"current"`
    			Maximum int `json:"maximum"`
    		} `json:"activeForms"`
    	} `json:"limits"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(len(item.EntitySchemes))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The example is shortened: only a few elements are left in the arrays.

```json
{
    "result": {
        "item": {
            "entitySchemes": [
                {
                    "id": 1,
                    "name": "Lead",
                    "description": "lead",
                    "entityNames": ["LEAD", "COMPANY", "CONTACT"],
                    "mainEntityName": "LEAD",
                    "hasInvoice": false,
                    "dynamic": false
                },
                {
                    "id": 2,
                    "name": "Client",
                    "description": "contact or company",
                    "entityNames": ["CONTACT", "COMPANY"],
                    "mainEntityName": null,
                    "hasInvoice": false,
                    "dynamic": false
                },
                {
                    "id": 3,
                    "name": "Deal",
                    "description": "deal, contact or company",
                    "entityNames": ["DEAL", "COMPANY", "CONTACT"],
                    "mainEntityName": "DEAL",
                    "hasInvoice": false,
                    "dynamic": false
                },
                {
                    "id": 1770,
                    "name": "Equipment Procurement",
                    "description": "Smart Process Automation, contact or company",
                    "entityNames": ["DYNAMIC_177", "COMPANY", "CONTACT"],
                    "mainEntityName": "DYNAMIC_177",
                    "hasInvoice": false,
                    "dynamic": true
                }
            ],
            "duplicateModes": [
                {
                    "id": "MERGE",
                    "name": "Merge"
                },
                {
                    "id": "REPLACE",
                    "name": "Replace"
                }
            ],
            "lead": {
                "enabled": true
            },
            "dealCategories": [
                {
                    "id": 0,
                    "name": "General"
                },
                {
                    "id": 1,
                    "name": "Newest"
                }
            ],
            "smartProcesses": [
                {
                    "entityTypeId": 177,
                    "entityName": "DYNAMIC_177",
                    "name": "Equipment Procurement",
                    "categories": [
                        {
                            "id": 7,
                            "name": "General"
                        },
                        {
                            "id": 9,
                            "name": "Second Pipeline"
                        }
                    ]
                }
            ],
            "languages": [
                {
                    "id": "en",
                    "name": "English"
                },
                {
                    "id": "de",
                    "name": "German"
                }
            ],
            "currencies": [
                {
                    "id": "EUR",
                    "fullName": "Euro",
                    "formatString": "&euro;#"
                }
            ],
            "fieldTypes": [
                {
                    "id": "section",
                    "name": "Section",
                    "parameters": ["label"]
                },
                {
                    "id": "phone",
                    "name": "Phone",
                    "parameters": ["label", "required", "multiple", "placeholder", "valueType", "value"]
                },
                {
                    "id": "hr",
                    "name": "Line break",
                    "parameters": []
                }
            ],
            "paySystems": [
                {
                    "id": 11,
                    "name": "Cash"
                },
                {
                    "id": 31,
                    "name": "Card payment"
                }
            ],
            "permissions": {
                "canRead": true,
                "canEdit": true
            },
            "limits": {
                "activeForms": {
                    "current": 4,
                    "maximum": 99999
                }
            },
            "product": {
                "isCloud": true
            },
            "region": "de"
        }
    },
    "time": {
        "start": 1792593640,
        "finish": 1792593640.689641,
        "duration": 0.689640998840332,
        "processing": 0,
        "date_start": "2026-10-21T17:40:40+02:00",
        "date_finish": "2026-10-21T17:40:40+02:00",
        "operating_reset_at": 1792593600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **item**
[`object`](../../../data-types.md) | Object with the form builder configuration [(detailed description)](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **entitySchemes**
[`object[]`](../../../data-types.md) | Schemes for creating CRM objects: which objects a submitted form creates [(detailed description)](#entity-schemes).

The scheme identifier is passed in the `crm.entityScheme` field when creating and updating a form ||
|| **duplicateModes**
[`object[]`](../../../data-types.md) | Modes for handling repeated form submissions: elements with the `id` and `name` fields.

The `id` value is passed in the `crm.duplicateMode` field. On a test Bitrix24 account, `MERGE` — merge and `REPLACE` — replace are returned ||
|| **lead**
[`object`](../../../data-types.md) | Object with the `enabled` field of type `boolean`:
- `true` — leads are enabled in CRM
- `false` — CRM operates in simple mode without leads ||
|| **dealCategories**
[`object[]`](../../../data-types.md) | Deal pipelines: elements with the `id` and `name` fields.

The `id` value is passed in the `crm.dealCategoryId` field. The general pipeline has the identifier `0` ||
|| **smartProcesses**
[`object[]`](../../../data-types.md) | SPAs whose items the form can create [(detailed description)](#smart-processes) ||
|| **languages**
[`object[]`](../../../data-types.md) | Form languages: elements with the `id` and `name` fields.

The `id` value is passed in the `languageId` field ||
|| **currencies**
[`object[]`](../../../data-types.md) | Bitrix24 currencies [(detailed description)](#currencies) ||
|| **fieldTypes**
[`object[]`](../../../data-types.md) | Field types a form can be built from [(detailed description)](#field-types).

The `id` value is passed in the `fields[].type` field ||
|| **paySystems**
[`object[]`](../../../data-types.md) | Payment systems that can be used to pay for a submitted form: elements with the `id` and `name` fields.

The identifiers of systems the form should not offer are passed in the `payment.disabledPaySystemsId` field. If the `sale` module is not installed in Bitrix24, the array is empty ||
|| **permissions**
[`object`](../../../data-types.md) | Permissions of the current user for CRM forms:
- `canRead` — can view forms
- `canEdit` — can create, update, and delete forms ||
|| **limits**
[`object`](../../../data-types.md) | Object with the `activeForms` field — the active forms limit:
- `current` — how many forms are currently active
- `maximum` — how many forms can be kept active on the plan

If the number of active forms in Bitrix24 is not limited, `activeForms` is `null` ||
|| **product**
[`object`](../../../data-types.md) | Product information:
- `isCloud` — cloud Bitrix24 ||
|| **region**
[`string`](../../../data-types.md) | Bitrix24 region code, for example `de` ||
|#

#### entitySchemes Element {#entity-schemes}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Scheme identifier ||
|| **name**
[`string`](../../../data-types.md) | Scheme name ||
|| **description**
[`string`](../../../data-types.md) | Scheme description: which objects it creates ||
|| **entityNames**
[`string[]`](../../../data-types.md) | Codes of the CRM objects the scheme creates, for example `LEAD`, `CONTACT`, `COMPANY`, `DYNAMIC_174`.

The codes are passed to the filter of the [crm.form.entityfield.list](./crm-form-entityfield-list.md) method to retrieve fields for the form ||
|| **mainEntityName**
[`string`](../../../data-types.md) | Code of the main object of the scheme. For the "Client" scheme — `null` ||
|| **hasInvoice**
[`boolean`](../../../data-types.md) | Indicates that the scheme creates an invoice ||
|| **dynamic**
[`boolean`](../../../data-types.md) | Indicates an SPA scheme ||
|#

#### smartProcesses Element {#smart-processes}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | SPA type identifier ||
|| **entityName**
[`string`](../../../data-types.md) | SPA code, for example `DYNAMIC_177` ||
|| **name**
[`string`](../../../data-types.md) | SPA name ||
|| **categories**
[`object[]`](../../../data-types.md) | SPA pipelines: elements with the `id` and `name` fields.

The `id` value is passed in the `crm.smartProcessCategoryId` field ||
|#

#### currencies Element {#currencies}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Currency code, for example `EUR` ||
|| **fullName**
[`string`](../../../data-types.md) | Currency name ||
|| **formatString**
[`string`](../../../data-types.md) | Amount display template; `#` is replaced with the amount ||
|#

#### fieldTypes Element {#field-types}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Form field type code, for example `string`, `phone`, `email`, `section`, `hr` ||
|| **name**
[`string`](../../../data-types.md) | Field type name ||
|| **parameters**
[`string[]`](../../../data-types.md) | Parameters that can be set for a field of this type in the `fields[]` element, for example `label`, `required`, `multiple`, `placeholder`, `valueType`, `value`. For the `hr` and `br` separators, the array is empty ||
|#

{% note info "" %}

The `fieldTypes` directory contains no field types with options: lists, checkboxes, radio buttons. Such fields, as well as products and booking, cannot yet be added to a form via REST.

{% endnote %}

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION",
        "message": "Access denied"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Grant the user permission to view CRM forms ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-entityfield-list.md)
- [{#T}](./crm-form-scenario-list.md)
- [{#T}](../crm-form-add.md)
- [{#T}](./crm-form-configuration-field-list.md)
- [{#T}](./index.md)
