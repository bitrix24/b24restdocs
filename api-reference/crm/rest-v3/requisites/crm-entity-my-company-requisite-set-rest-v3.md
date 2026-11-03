# Select My Company Requisites in an Item v 3.0 crm.[type].mycompanyrequisite.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.mycompanyrequisite.set` method selects my company requisite and bank details in a deal or invoice and returns the selected pair. Only a requisite of the my company specified in the `myCompanyId` field of the item can be selected.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.mycompanyrequisite.set` ||
|| Invoice | `crm.smartinvoice.mycompanyrequisite.set` ||
|#

The methods exist only for deals and invoices. My company requisites are returned by the [crm.company.requisite.list](./crm-entity-requisite-list-rest-v3.md) method with the my company ID in the `companyId` filter.

The method name is written in lowercase: `crm.smartinvoice.mycompanyrequisite.set`, not `crm.smartInvoice.myCompanyRequisite.set`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Deal or invoice ID.

The ID can be retrieved with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Selected requisite pair:

- `requisiteId`* — ID of a requisite of the my company from the `myCompanyId` field of the item. `0` clears the selected requisite and bank details
- `bankRequisiteId` — ID of the bank details of the selected requisite. My company bank details are returned by the [crm.company.bankrequisite.list](./crm-entity-bank-requisite-list-rest-v3.md) method

The pair is written as a whole: if `bankRequisiteId` is not passed or is `0`, the selected bank details are cleared ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.deal.mycompanyrequisite.set`

{% endnote %}

The examples select my company requisite `3` and its bank details `3` in deal `9003`. For an invoice, replace the method name with `crm.smartinvoice.mycompanyrequisite.set`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9003,"fields":{"requisiteId":3,"bankRequisiteId":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.mycompanyrequisite.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9003,"fields":{"requisiteId":3,"bankRequisiteId":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.deal.mycompanyrequisite.set
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MyCompanyRequisiteSetResult = {
      item: {
        requisiteId: number | null
        bankRequisiteId: number | null
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<MyCompanyRequisiteSetResult>(
        'crm.deal.mycompanyrequisite.set',
        {
          id: 9003,
          fields: {
            requisiteId: 3,
            bankRequisiteId: 3,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('My company requisite saved:', result.item)
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
      async function setMyCompanyRequisite() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.deal.mycompanyrequisite.set',
            {
              id: 9003,
              fields: {
                requisiteId: 3,
                bankRequisiteId: 3,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('My company requisite saved:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setMyCompanyRequisite)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.deal.mycompanyrequisite.set"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 9003,
                "fields": {
                    "requisiteId": 3,
                    "bankRequisiteId": 3,
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
                'crm.deal.mycompanyrequisite.set',
                [
                    'id' => 9003,
                    'fields' => [
                        'requisiteId' => 3,
                        'bankRequisiteId' => 3,
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
        'crm.deal.mycompanyrequisite.set',
        {
            id: 9003,
            fields: {
                requisiteId: 3,
                bankRequisiteId: 3,
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
        'crm.deal.mycompanyrequisite.set',
        [
            'id' => 9003,
            'fields' => [
                'requisiteId' => 3,
                'bankRequisiteId' => 3,
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
    res, err := client.Core().Call(ctx, "crm.deal.mycompanyrequisite.set", b24.Params{
    	"id": 9003,
    	"fields": b24.Params{
    		"requisiteId":     3,
    		"bankRequisiteId": 3,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.deal.mycompanyrequisite.set: %w", err)
    }

    var result struct {
    	Item struct {
    		RequisiteID     *int `json:"requisiteId"`
    		BankRequisiteID *int `json:"bankRequisiteId"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.RequisiteID, result.Item.BankRequisiteID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "requisiteId": 3,
            "bankRequisiteId": 3
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
|| **result.item.requisiteId**
[`integer`](../../../data-types.md) | ID of the selected requisite after the write. `null` if the selection is cleared ||
|| **result.item.bankRequisiteId**
[`integer`](../../../data-types.md) | ID of the selected bank details after the write. `null` if no bank details are selected ||
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
                "message": "Only \"Your company details\" can be selected.",
                "field": "fields.requisiteId"
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
|| `id` | Required field `id` is missing | Pass the deal or invoice `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `fields` | The fields property must not be empty | Pass `fields` with `requisiteId` ||
|| `fields.requisiteId` | This request requires data type `integer` in field `fields.requisiteId` | Pass the ID as a number ||
|| `fields.requisiteId` | Only "Your company details" can be selected | Select a requisite of the company from the `myCompanyId` field of the item ||
|| `fields.bankRequisiteId` | The banking details do not belong to the selected company details | Pass bank details that belong to `requisiteId` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `requisiteId` | Required field `requisiteId` is empty | Pass `requisiteId` in `fields` ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | The deal or invoice does not exist, or you have no permission to edit it. Check `id` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `MyCompanyRequisiteDto` | Pass only `requisiteId` and `bankRequisiteId` in `fields` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.mycompanyrequisite.set`. The methods exist only for deals and invoices: `crm.deal.mycompanyrequisite.*` and `crm.smartinvoice.mycompanyrequisite.*` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-my-company-requisite-get-rest-v3.md)
- [{#T}](./crm-entity-client-requisite-set-rest-v3.md)
