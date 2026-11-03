# Get a Requisite Address v 3.0 crm.[type].requisite.address.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the contact or company that owns the requisite

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.requisite.address.get` method retrieves the requisite address of the specified type.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Company | `crm.company.requisite.address.get` ||
|| Contact | `crm.contact.requisite.address.get` ||
|#

A requisite address is identified by the pair of `requisiteId` and `typeId`: a requisite can have only one address of each type, and the address has no separate ID. The `company` or `contact` segment of the method name must match the owner of the requisite.

The method name is written in lowercase: `crm.company.requisite.address.get`, not `crm.company.requisite.Address.Get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **requisiteId***
[`integer`](../../../data-types.md) | Requisite ID.

You can retrieve the ID with the [crm.&lt;type&gt;.requisite.list](./crm-entity-requisite-list-rest-v3.md) method ||
|| **typeId***
[`integer`](../../../data-types.md) | Address type, for example, `1` — actual address, `6` — legal address. The list of types is in the [requisite methods overview](./index.md#address-fields) ||
|| **select**
[`array`](../../../data-types.md) | List of address fields to return, for example, `["city","postalCode"]`. [Field descriptions](./index.md#address-fields).

Default: all address fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.company.requisite.address.get`

{% endnote %}

The examples retrieve the legal address of requisite `549`. For a contact, replace the method name with `crm.contact.requisite.address.get`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"requisiteId":549,"typeId":6,"select":["typeId","address1","city","postalCode"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.requisite.address.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"requisiteId":549,"typeId":6,"select":["typeId","address1","city","postalCode"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.company.requisite.address.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RequisiteAddressGetResult = {
      item: {
        typeId: number
        address1: string
        city: string
        postalCode: string
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RequisiteAddressGetResult>(
        'crm.company.requisite.address.get',
        {
          requisiteId: 549,
          typeId: 6,
          select: ['typeId', 'address1', 'city', 'postalCode'],
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
      async function getRequisiteAddress() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.company.requisite.address.get',
            {
              requisiteId: 549,
              typeId: 6,
              select: ['typeId', 'address1', 'city', 'postalCode'],
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

      document.addEventListener('DOMContentLoaded', getRequisiteAddress)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.company.requisite.address.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "requisiteId": 549,
                "typeId": 6,
                "select": ["typeId", "address1", "city", "postalCode"],
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
                'crm.company.requisite.address.get',
                [
                    'requisiteId' => 549,
                    'typeId' => 6,
                    'select' => ['typeId', 'address1', 'city', 'postalCode'],
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
        'crm.company.requisite.address.get',
        {
            requisiteId: 549,
            typeId: 6,
            select: ['typeId', 'address1', 'city', 'postalCode'],
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
        'crm.company.requisite.address.get',
        [
            'requisiteId' => 549,
            'typeId' => 6,
            'select' => ['typeId', 'address1', 'city', 'postalCode'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.company.requisite.address.get", b24.Params{
    	"requisiteId": 549,
    	"typeId":      6,
    	"select":      []string{"typeId", "address1", "city", "postalCode"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.company.requisite.address.get: %w", err)
    }

    var result struct {
    	Item struct {
    		TypeID     int    `json:"typeId"`
    		Address1   string `json:"address1"`
    		City       string `json:"city"`
    		PostalCode string `json:"postalCode"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.TypeID, result.Item.City, result.Item.Address1)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "typeId": 6,
            "address1": "Friedrichstraße 10",
            "city": "Berlin",
            "postalCode": "10117"
        }
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
|| **result.item**
[`object`](../../../data-types.md) | Address with the fields from `select`. Empty fields are returned as an empty string. [Field descriptions](./index.md#address-fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 549 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `requisiteId` | Required field `requisiteId` is missing | Pass `requisiteId` ||
|| `typeId` | Required field `typeId` is missing | Pass `typeId` ||
|#

#### Address or Requisite Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The requisite has no address of this type, the requisite does not exist, it belongs to an owner of a different type, or you have no permissions for the owner. The error message contains `requisiteId` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.requisite.address.get`. Requisite methods exist only for contacts and companies: the `company` or `contact` segment ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-requisite-address-list-rest-v3.md)
- [{#T}](./crm-entity-requisite-address-update-rest-v3.md)
