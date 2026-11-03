# Get an Address v 3.0 crm.[type].address.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the contact, company, or lead to which the address belongs

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.address.get` method returns a contact, company, or lead address. The owner type is set by the `<type>` segment of the method name. For a contact or company, the method retrieves the address by its ID, and for a lead, by the lead ID.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Contact | `crm.contact.address.get` ||
|| Company | `crm.company.address.get` ||
|| Lead | `crm.lead.address.get` ||
|#

The method name is written in lowercase: `crm.company.address.get`, not `crm.Company.address.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID. The value depends on the owner type:

- for a contact or company, the address ID. It can be retrieved with the [crm.&lt;type&gt;.address.list](./crm-entity-address-list-rest-v3.md) method. The address must belong to an owner of the type set in the method name
- for a lead, the lead ID. It can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|#

The method returns all address fields. The `select` parameter does not affect the response.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.contact.address.get`

{% endnote %}

The examples retrieve a contact address. For a company or a lead, replace the method name according to the table above. For a lead, pass the lead ID in `id`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":633}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.address.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":633,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.contact.address.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AddressGetResult = {
      item: {
        id: number
        typeId: number
        contactId: number
        requisiteId: number
        address1: string
        city: string
        postalCode: string
        country: string
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<AddressGetResult>(
        'crm.contact.address.get',
        {
          id: 633,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Address:', result.item)
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
      async function getAddress() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.contact.address.get',
            {
              id: 633,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Address:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAddress)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.contact.address.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 633,
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
                'crm.contact.address.get',
                [
                    'id' => 633,
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
        'crm.contact.address.get',
        {
            id: 633,
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
        'crm.contact.address.get',
        [
            'id' => 633,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.contact.address.get", b24.Params{
    	"id": 633,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.address.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID          int    `json:"id"`
    		TypeID      int    `json:"typeId"`
    		ContactID   int    `json:"contactId"`
    		RequisiteID int    `json:"requisiteId"`
    		Address1    string `json:"address1"`
    		City        string `json:"city"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.TypeID, result.Item.City, result.Item.Address1)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

{% list tabs %}

- Contact and company

    ```json
    {
        "result": {
            "item": {
                "id": 633,
                "typeId": 1,
                "contactId": 3219,
                "requisiteId": 547,
                "address1": "Friedrichstraße 1",
                "address2": "",
                "city": "Berlin",
                "postalCode": "10117",
                "region": "",
                "province": "",
                "country": "Germany",
                "countryCode": ""
            }
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

- Lead

    ```json
    {
        "result": {
            "item": {
                "address1": "Friedrichstraße 1",
                "address2": "",
                "city": "Berlin",
                "postalCode": "10117",
                "region": "",
                "province": "",
                "country": "Germany",
                "countryCode": ""
            }
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

{% endlist %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.item**
[`object`](../../../data-types.md) | Address fields. A company address contains `companyId` instead of `contactId`. A lead address has no `id`, `typeId`, or `requisiteId` fields. If the lead address is not filled in, all fields are returned as empty strings. [Description of address fields](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 633 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|#

#### Address Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | For a contact or company: the address does not exist, belongs to an owner of a different type, or you do not have permission to view the owner. For a lead: the lead does not exist, or you do not have permission to view it. Check `id` and the method name ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.address.get`. The method exists only for contacts, companies, and leads ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-address-list-rest-v3.md)
- [{#T}](./crm-entity-address-update-rest-v3.md)
- [{#T}](./crm-lead-address-set-rest-v3.md)
