# Add a Requisite v 3.0 crm.[type].requisite.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the contact or company to which the requisite is added

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.requisite.add` method adds a requisite to a contact or company and returns its ID. The owner type is set by the `<type>` segment of the method name: `company` or `contact`.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Company | `crm.company.requisite.add` ||
|| Contact | `crm.contact.requisite.add` ||
|#

Only contacts and companies have requisites. The owner type is set by the `company` or `contact` segment of the method name; the method does not find a requisite of a different owner type. My company requisites are available through the `crm.company.*` methods.

The method name is written in lowercase: `crm.company.requisite.add`, not `crm.Company.Requisite.Add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the requisite fields in the format `{"field": "value"}`. [Parameter description](#fields)

A requisite has [required fields](#required-fields) ||
|#

### The fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **companyId** or **contactId***
[`integer`](../../../data-types.md) | ID of the company or contact to which the requisite is added. The field name depends on the method: `companyId` in `crm.company.requisite.add`, `contactId` in `crm.contact.requisite.add`. The owner cannot be changed after creation.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **presetId***
[`integer`](../../../data-types.md) | Requisite template ID. The template defines which fields can be filled in: organization, sole proprietor, person, and others. The set of templates depends on the country, for example, the German templates are "Unternehmen" and "Natürliche Person".

Templates and their fields are returned by the [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method ||
|| **name***
[`string`](../../../data-types.md) | Requisite name, for example, "Main requisite" ||
|| **active**
[`boolean`](../../../data-types.md) | Whether the requisite is active.

Possible values:

- `true` — active
- `false` — inactive

Default: `true` ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order.

Default: `500` ||
|| **xmlId**
[`string`](../../../data-types.md) | External requisite code ||
|| **originatorId**
[`string`](../../../data-types.md) | ID of the external data source ||
|| Requisite data fields | For example, `companyTitle`, `inn`, `vatNumber`, `usrle`, `firstName`, `lastName`. Only the fields included in the `presetId` template can be passed: they are listed in the `fields` array of the template with `kind: "requisiteField"`. The fields are described in the [requisite methods overview](./index.md#fields) ||
|#

Addresses and bank details are not passed in `fields`. Add them after creating the requisite with the [crm.&lt;type&gt;.requisite.address.add](./crm-entity-requisite-address-add-rest-v3.md) and [crm.&lt;type&gt;.requisite.bankrequisite.add](./crm-entity-requisite-bank-requisite-add-rest-v3.md) methods.

{% note warning "" %}

Pass a non-empty `name`. Without `name`, the method does not respond and the request times out; with an empty string, it returns HTTP 500 `INTERNAL_SERVER_ERROR`. In both cases, the requisite is not created.

{% endnote %}

### Required Fields {#required-fields}

Without these fields, the method returns an error and does not create the requisite:

#|
|| **Object type** | **Required** ||
|| Company | `companyId`, `presetId`, and a non-empty `name` ||
|| Contact | `contactId`, `presetId`, and a non-empty `name` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.company.requisite.add`

{% endnote %}

The examples add a company requisite to the company with the ID `3167`. The examples use the German "Unternehmen" template. On your Bitrix24, retrieve the template ID with the [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method. For a contact, replace the method name with `crm.contact.requisite.add` and `companyId` with `contactId`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"companyId":3167,"presetId":1,"name":"Main requisite","companyTitle":"Müller GmbH","inn":"21/815/08150","vatNumber":"DE123456789","usrle":"HRB 123456"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.requisite.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"companyId":3167,"presetId":1,"name":"Main requisite","companyTitle":"Müller GmbH","inn":"21/815/08150","vatNumber":"DE123456789","usrle":"HRB 123456"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.company.requisite.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RequisiteAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RequisiteAddResult>(
        'crm.company.requisite.add',
        {
          fields: {
            companyId: 3167,
            presetId: 1,
            name: 'Main requisite',
            companyTitle: 'Müller GmbH',
            inn: '21/815/08150',
            vatNumber: 'DE123456789',
            usrle: 'HRB 123456',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Requisite created:', result.id)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addRequisite() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.company.requisite.add',
            {
              fields: {
                companyId: 3167,
                presetId: 1,
                name: 'Main requisite',
                companyTitle: 'Müller GmbH',
                inn: '21/815/08150',
                vatNumber: 'DE123456789',
                usrle: 'HRB 123456',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Requisite created:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addRequisite)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.company.requisite.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "companyId": 3167,
                    "presetId": 1,
                    "name": "Main requisite",
                    "companyTitle": "Müller GmbH",
                    "inn": "21/815/08150",
                    "vatNumber": "DE123456789",
                    "usrle": "HRB 123456",
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.requisite.add',
                [
                    'fields' => [
                        'companyId' => 3167,
                        'presetId' => 1,
                        'name' => 'Main requisite',
                        'companyTitle' => 'Müller GmbH',
                        'inn' => '21/815/08150',
                        'vatNumber' => 'DE123456789',
                        'usrle' => 'HRB 123456',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.company.requisite.add',
        {
            fields: {
                companyId: 3167,
                presetId: 1,
                name: 'Main requisite',
                companyTitle: 'Müller GmbH',
                inn: '21/815/08150',
                vatNumber: 'DE123456789',
                usrle: 'HRB 123456',
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.requisite.add',
        [
            'fields' => [
                'companyId' => 3167,
                'presetId' => 1,
                'name' => 'Main requisite',
                'companyTitle' => 'Müller GmbH',
                'inn' => '21/815/08150',
                'vatNumber' => 'DE123456789',
                'usrle' => 'HRB 123456',
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
    res, err := client.Core().Call(ctx, "crm.company.requisite.add", b24.Params{
    	"fields": b24.Params{
    		"companyId":    3167,
    		"presetId":     1,
    		"name":         "Main requisite",
    		"companyTitle": "Müller GmbH",
    		"inn":          "21/815/08150",
    		"vatNumber":    "DE123456789",
    		"usrle":        "HRB 123456",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.company.requisite.add: %w", err)
    }

    var result struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 549
    },
    "time": {
        "start": 1793711900,
        "finish": 1793711900.089311,
        "duration": 0.08931088447570801,
        "processing": 0,
        "date_start": "2026-11-03T16:18:20+03:00",
        "date_finish": "2026-11-03T16:18:20+03:00",
        "operating_reset_at": 1793712480,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.id**
[`integer`](../../../data-types.md) | ID of the created requisite. The requisite fields can be retrieved with the [crm.&lt;type&gt;.requisite.get](./crm-entity-requisite-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "This field is not included in the details template.",
                "field": "firstName"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `presetId` | Details template was not found | Pass a template from the [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method response ||
|| `firstName` | This field is not included in the details template | Pass only the fields listed in `fields` of the `presetId` template. The template fields are returned by the [crm.&lt;type&gt;.requisite.preset.get](./crm-entity-requisite-preset-get-rest-v3.md) method ||
|| `inn` | The field value must not exceed `#MAX_LENGTH#` characters | Shorten the value to the specified length ||
|| `vatPayer` | This request requires data type `bool` in field `vatPayer` | Pass a value of the type specified in the field description. Boolean fields accept `true` or `false` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `companyId` | Required field `companyId` is empty | Pass the owner: `companyId` for a company, `contactId` for a contact ||
|| `presetId` | Required field `presetId` is empty | Pass `presetId` ||
|| `createdById` | Field `createdById` is read-only | Remove the read-only field from `fields`. The method returns the same error for `id`, `createdTime`, `updatedTime`, `updatedById`, `addresses`, and `bankRequisites` ||
|#

#### Owner Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | The contact or company does not exist, or you have no permission to edit it. Check the owner ID and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `OwnerRequisiteCompanyDto` | Check the field name against the [requisite field description](./index.md#fields). The owner is `companyId` in company methods and `contactId` in contact methods ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.requisite.add`. Requisite methods exist only for contacts and companies: the `company` or `contact` segment ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-requisite-update-rest-v3.md)
- [{#T}](./crm-entity-requisite-preset-list-rest-v3.md)
- [{#T}](./crm-entity-requisite-bank-requisite-add-rest-v3.md)
- [{#T}](./crm-entity-requisite-address-add-rest-v3.md)
