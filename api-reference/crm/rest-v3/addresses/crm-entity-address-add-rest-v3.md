# Add a Contact or Company Address v 3.0 crm.[type].address.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the contact or company to which the address is added

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.address.add` method adds an address to a contact or company and returns the address ID. The owner type is set by the `<type>` segment of the method name. The address is stored in an owner's requisite: if there are no requisites, CRM creates a requisite to store the address.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Contact | `crm.contact.address.add` ||
|| Company | `crm.company.address.add` ||
|#

A lead address is set by the [crm.lead.address.set](./crm-lead-address-set-rest-v3.md) method.

The method name is written in lowercase: `crm.company.address.add`, not `crm.Company.address.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the address fields in the format `{"field": "value"}`. [Parameter description](#fields)

An address has [required fields](#required-fields) ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **contactId***
[`integer`](../../../data-types.md) | Contact ID. Passed in `crm.contact.address.add`.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **companyId***
[`integer`](../../../data-types.md) | Company ID. Passed in `crm.company.address.add`.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **typeId***
[`integer`](../../../data-types.md) | Address type.

Possible values:

- `1` — street address
- `4` — registered address
- `6` — legal address
- `9` — beneficiary address
- `11` — delivery address

The list of types is returned by the [crm.dictionary.addresstype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method. An owner can have only one address of each type ||
|| **address1**
[`string`](../../../data-types.md) | Street, building, block, structure ||
|| **address2**
[`string`](../../../data-types.md) | Apartment or office ||
|| **city**
[`string`](../../../data-types.md) | City ||
|| **postalCode**
[`string`](../../../data-types.md) | Postal code ||
|| **region**
[`string`](../../../data-types.md) | District ||
|| **province**
[`string`](../../../data-types.md) | Region ||
|| **country**
[`string`](../../../data-types.md) | Country ||
|| **countryCode**
[`string`](../../../data-types.md) | Country code from the Bitrix24 locations reference. The code is checked against the country name in `country`: if the name does not match the country name in the Bitrix24 language, the method returns an error. If you do not have a country code, do not pass the field ||
|#

The `id` and `requisiteId` fields are read-only. CRM chooses the requisite for the address itself: a requisite with an address of the same type, the requisite selected in the owner form, or the first requisite. To add an address to a specific requisite, use the [crm.&lt;type&gt;.requisite.address.add](../requisites/crm-entity-requisite-address-add-rest-v3.md) method.

### Required Fields {#required-fields}

Without these fields, the method returns an error and does not create the address:

#|
|| **Object type** | **Required** ||
|| Contact | `contactId`, `typeId`, and at least one address field: `address1`, `address2`, `city`, `postalCode`, `region`, `province`, or `country` ||
|| Company | `companyId`, `typeId`, and at least one address field: `address1`, `address2`, `city`, `postalCode`, `region`, `province`, or `country` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.contact.address.add`

{% endnote %}

The examples add a street address to a contact. For a company, replace the method name according to the table above and pass `companyId` instead of `contactId`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"contactId":3219,"typeId":1,"address1":"Friedrichstraße 1","city":"Berlin","postalCode":"10117","country":"Germany"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.address.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"contactId":3219,"typeId":1,"address1":"Friedrichstraße 1","city":"Berlin","postalCode":"10117","country":"Germany"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.contact.address.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AddressAddResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<AddressAddResult>(
        'crm.contact.address.add',
        {
          fields: {
            contactId: 3219,
            typeId: 1,
            address1: 'Friedrichstraße 1',
            city: 'Berlin',
            postalCode: '10117',
            country: 'Germany',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Address created:', result.id)
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
      async function addAddress() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.contact.address.add',
            {
              fields: {
                contactId: 3219,
                typeId: 1,
                address1: 'Friedrichstraße 1',
                city: 'Berlin',
                postalCode: '10117',
                country: 'Germany',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Address created:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addAddress)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.contact.address.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "contactId": 3219,
                    "typeId": 1,
                    "address1": "Friedrichstraße 1",
                    "city": "Berlin",
                    "postalCode": "10117",
                    "country": "Germany",
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
                'crm.contact.address.add',
                [
                    'fields' => [
                        'contactId' => 3219,
                        'typeId' => 1,
                        'address1' => 'Friedrichstraße 1',
                        'city' => 'Berlin',
                        'postalCode' => '10117',
                        'country' => 'Germany',
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
        'crm.contact.address.add',
        {
            fields: {
                contactId: 3219,
                typeId: 1,
                address1: 'Friedrichstraße 1',
                city: 'Berlin',
                postalCode: '10117',
                country: 'Germany',
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
        'crm.contact.address.add',
        [
            'fields' => [
                'contactId' => 3219,
                'typeId' => 1,
                'address1' => 'Friedrichstraße 1',
                'city' => 'Berlin',
                'postalCode' => '10117',
                'country' => 'Germany',
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
    res, err := client.Core().Call(ctx, "crm.contact.address.add", b24.Params{
    	"fields": b24.Params{
    		"contactId":  3219,
    		"typeId":     1,
    		"address1":   "Friedrichstraße 1",
    		"city":       "Berlin",
    		"postalCode": "10117",
    		"country":    "Germany",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.contact.address.add: %w", err)
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
        "id": 633
    },
    "time": {
        "start": 1793709949,
        "finish": 1793709949.853585,
        "duration": 0.8535850048065186,
        "processing": 0,
        "date_start": "2026-11-03T15:45:49+03:00",
        "date_finish": "2026-11-03T15:45:49+03:00",
        "operating_reset_at": 1793710260,
        "operating": 0.3442521095275879
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
[`integer`](../../../data-types.md) | ID of the created address. The address and the requisite in which it is stored can be retrieved with the [crm.&lt;type&gt;.address.get](./crm-entity-address-get-rest-v3.md) method ||
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
                "message": "An address of this type already exists. Select another type.",
                "field": "typeId"
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
|| `fields` | Fill in at least one address field | Pass at least one of the `address1`, `address2`, `city`, `postalCode`, `region`, `province`, or `country` fields ||
|| `typeId` | An address of this type already exists. Select another type | Update the existing address with the [crm.&lt;type&gt;.address.update](./crm-entity-address-update-rest-v3.md) method or pass a different `typeId` ||
|| `typeId` | Unknown address type. Check the value | Pass a type from [crm.dictionary.addresstype.list](../dictionaries/crm-dictionary-list-rest-v3.md) ||
|| `countryCode` | The country code does not match the country name. Check both fields | Pass the country name in the Bitrix24 language in `country`, or do not pass `countryCode` ||
|| `contactId` | The details to store the address could not be created | The owner has no requisites, and CRM could not create one. Create a requisite with the [crm.&lt;type&gt;.requisite.add](../requisites/crm-entity-requisite-add-rest-v3.md) method and repeat the request. In `crm.company.address.add`, the field is `companyId` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `contactId` | Required field `contactId` is empty | Pass the owner ID: `contactId` for a contact, `companyId` for a company ||
|| `typeId` | Required field `typeId` is empty | Pass the address type ||
|| `requisiteId` | Field `requisiteId` is read-only | Remove the read-only field from `fields`. To add an address to a specific requisite, use [crm.&lt;type&gt;.requisite.address.add](../requisites/crm-entity-requisite-address-add-rest-v3.md) ||
|#

#### Owner Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | There is no contact or company with this ID, or you do not have permission to edit it. Check the owner ID and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `OwnerAddressContactDto` | Check the field name against the [fields parameter](#fields) table. The contact method does not accept `companyId`, and the company method does not accept `contactId`. For a company, the error message contains `OwnerAddressCompanyDto` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.address.add`. The method exists only for contacts and companies ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-address-update-rest-v3.md)
- [{#T}](./crm-entity-address-list-rest-v3.md)
- [{#T}](./crm-entity-address-delete-rest-v3.md)
- [{#T}](../requisites/crm-entity-requisite-address-add-rest-v3.md)
