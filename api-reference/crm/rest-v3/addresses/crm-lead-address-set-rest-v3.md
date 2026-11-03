# Set a Lead Address v 3.0 crm.lead.address.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the lead

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.lead.address.set` method sets a lead address. A lead has one address without a type or a separate ID, so the method both creates and updates it. The method changes only the passed fields, and the others are retained.

A lead address is returned by the [crm.lead.address.get](./crm-entity-address-get-rest-v3.md) method and deleted by [crm.lead.address.delete](./crm-entity-address-delete-rest-v3.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Lead ID.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with the address fields in the format `{"field": "value"}`. Pass at least one field. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
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
[`string`](../../../data-types.md) | Country code from the Bitrix24 locations reference. It is retained only if it matches the country name in `country` in the Bitrix24 language. Otherwise, the method saves the address without the country code and does not return an error ||
|#

An empty string clears a field. A lead address has no `typeId`, `id`, or `requisiteId` fields.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.lead.address.set`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1001357,"fields":{"address1":"Friedrichstraße 1","city":"Berlin","postalCode":"10117","country":"Germany"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.address.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1001357,"fields":{"address1":"Friedrichstraße 1","city":"Berlin","postalCode":"10117","country":"Germany"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.lead.address.set
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type LeadAddressSetResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<LeadAddressSetResult>(
        'crm.lead.address.set',
        {
          id: 1001357,
          fields: {
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
        console.info('Lead address saved:', result.result)
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
      async function setLeadAddress() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.lead.address.set',
            {
              id: 1001357,
              fields: {
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
          console.info('Lead address saved:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setLeadAddress)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.lead.address.set"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 1001357,
                "fields": {
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
                'crm.lead.address.set',
                [
                    'id' => 1001357,
                    'fields' => [
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
        'crm.lead.address.set',
        {
            id: 1001357,
            fields: {
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
        'crm.lead.address.set',
        [
            'id' => 1001357,
            'fields' => [
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
    res, err := client.Core().Call(ctx, "crm.lead.address.set", b24.Params{
    	"id": 1001357,
    	"fields": b24.Params{
    		"address1":   "Friedrichstraße 1",
    		"city":       "Berlin",
    		"postalCode": "10117",
    		"country":    "Germany",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.lead.address.set: %w", err)
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
[`boolean`](../../../data-types.md) | `true` if the address is set. The address can be retrieved with the [crm.lead.address.get](./crm-entity-address-get-rest-v3.md) method ||
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
                "message": "The fields property must not be empty.",
                "field": "fields"
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
|| `id` | Required field `id` is missing | Pass the lead `id` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `fields` | The fields property must not be empty | Pass at least one address field in `fields` ||
|#

#### Lead Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | There is no lead with this ID, or you do not have permission to edit it. Check `id` with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `AddressDto` | Check the field name against the [fields parameter](#fields) table. A lead address has no `typeId` field ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, `crm.lead.address.set`. The method exists only for leads, and contact and company addresses are added by [crm.&lt;type&gt;.address.add](./crm-entity-address-add-rest-v3.md) ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-address-get-rest-v3.md)
- [{#T}](./crm-entity-address-delete-rest-v3.md)
