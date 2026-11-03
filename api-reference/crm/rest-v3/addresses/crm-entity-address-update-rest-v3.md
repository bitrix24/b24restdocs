# Update a Contact or Company Address v 3.0 crm.[type].address.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the contact or company to which the address belongs

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.address.update` method updates the fields of a contact or company address. The owner type is set by the `<type>` segment of the method name. The method changes only the passed fields, and the others are retained.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Contact | `crm.contact.address.update` ||
|| Company | `crm.company.address.update` ||
|#

A lead address is updated by the [crm.lead.address.set](./crm-lead-address-set-rest-v3.md) method.

The method name is written in lowercase: `crm.company.address.update`, not `crm.Company.address.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Address ID.

The ID can be retrieved with the [crm.&lt;type&gt;.address.list](./crm-entity-address-list-rest-v3.md) method. The address must belong to an owner of the type set in the method name ||
|| **fields***
[`object`](../../../data-types.md) | Object with the new field values in the format `{"field": "value"}`.

You can change the `address1`, `address2`, `city`, `postalCode`, `region`, `province`, `country`, and `countryCode` fields. They are described in the [fields parameter table of the crm.&lt;type&gt;.address.add method](./crm-entity-address-add-rest-v3.md#fields). An empty string clears a field.

The address type `typeId` and the owner `contactId` or `companyId` cannot be changed. To change the type, add an address of the new type with the [crm.&lt;type&gt;.address.add](./crm-entity-address-add-rest-v3.md) method and delete the old one ||
|#

If `fields` is empty, the method returns `true` and changes nothing.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.contact.address.update`

{% endnote %}

The examples change the office and the postal code in a contact address. For a company, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":633,"fields":{"address2":"Office 10","postalCode":"10178"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.address.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":633,"fields":{"address2":"Office 10","postalCode":"10178"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.contact.address.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AddressUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<AddressUpdateResult>(
        'crm.contact.address.update',
        {
          id: 633,
          fields: {
            address2: 'Office 10',
            postalCode: '10178',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Address updated:', result.result)
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
      async function updateAddress() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.contact.address.update',
            {
              id: 633,
              fields: {
                address2: 'Office 10',
                postalCode: '10178',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Address updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateAddress)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.contact.address.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 633,
                "fields": {
                    "address2": "Office 10",
                    "postalCode": "10178",
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
                'crm.contact.address.update',
                [
                    'id' => 633,
                    'fields' => [
                        'address2' => 'Office 10',
                        'postalCode' => '10178',
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
        'crm.contact.address.update',
        {
            id: 633,
            fields: {
                address2: 'Office 10',
                postalCode: '10178',
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
        'crm.contact.address.update',
        [
            'id' => 633,
            'fields' => [
                'address2' => 'Office 10',
                'postalCode' => '10178',
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
    res, err := client.Core().Call(ctx, "crm.contact.address.update", b24.Params{
    	"id": 633,
    	"fields": b24.Params{
    		"address2":   "Office 10",
    		"postalCode": "10178",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.contact.address.update: %w", err)
    }

    var result struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
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
[`object`](../../../data-types.md) | Object with the operation result ||
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the address is updated. The new values can be retrieved with the [crm.&lt;type&gt;.address.get](./crm-entity-address-get-rest-v3.md) method ||
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
                "message": "The identity of an address is given when it is created and cannot be changed.",
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
|| `id` | Required field `id` is missing | Pass the address `id` ||
|| `typeId` | The identity of an address is given when it is created and cannot be changed | Remove `typeId` from `fields`. The method responds the same way to `contactId` and `companyId`. To change the type, add an address of the new type and delete the old one ||
|| `countryCode` | The country code does not match the country name. Check both fields | Pass the country name in the Bitrix24 language in `country`, or do not pass `countryCode` ||
|#

#### Address Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The address does not exist, belongs to an owner of a different type, or you do not have permission to edit the owner. Check `id` with the [crm.&lt;type&gt;.address.list](./crm-entity-address-list-rest-v3.md) method and check the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `OwnerAddressContactDto` | Check the field name against the [fields parameter](./crm-entity-address-add-rest-v3.md#fields) table. For a company, the error message contains `OwnerAddressCompanyDto` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.address.update`. The method exists only for contacts and companies ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-address-add-rest-v3.md)
- [{#T}](./crm-entity-address-get-rest-v3.md)
- [{#T}](./crm-entity-address-delete-rest-v3.md)
