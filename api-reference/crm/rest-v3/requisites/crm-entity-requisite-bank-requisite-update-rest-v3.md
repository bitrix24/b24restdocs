# Update a Bank Detail v 3.0 crm.[type].requisite.bankrequisite.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the contact or company that owns the requisite

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.requisite.bankrequisite.update` method updates the fields of a bank detail and returns it after saving. The method changes only the fields you pass.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Company | `crm.company.requisite.bankrequisite.update` ||
|| Contact | `crm.contact.requisite.bankrequisite.update` ||
|#

Bank details belong to requisites of contacts and companies. The `company` or `contact` segment of the method name must match the owner of the requisite: a `crm.contact.*` method does not find a bank detail of a company.

The method name is written in lowercase: `crm.company.requisite.bankrequisite.update`, not `crm.company.requisite.bankRequisite.update`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Bank detail ID.

You can retrieve the ID with the [crm.&lt;type&gt;.requisite.bankrequisite.list](./crm-entity-requisite-bank-requisite-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with the new field values. Pass at least one field.

You can change `name` and the bank fields. The set of fields depends on the country of the requisite template. For example, the German template allows `name`, `comments`, `bankName`, `bankAddress`, `bankRoutingNumber`, `bankAccountHolderName`, `bankAccountNumber`, `iban`, `swift`, and `bic`. The method rejects fields outside this set. All fields are described in the [requisite methods overview](./index.md#bank-fields).

The `requisiteId` field cannot be changed: a bank detail cannot be moved to another requisite ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.company.requisite.bankrequisite.update`

{% endnote %}

The examples add a comment and a bank address to bank detail `23`. For a contact, replace the method name with `crm.contact.requisite.bankrequisite.update`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":23,"fields":{"comments":"For supplier payments","bankAddress":"Kurfürstendamm 21, 10719 Berlin, Germany"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.requisite.bankrequisite.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":23,"fields":{"comments":"For supplier payments","bankAddress":"Kurfürstendamm 21, 10719 Berlin, Germany"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.company.requisite.bankrequisite.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type BankRequisiteUpdateResult = {
      item: {
        id: number
        requisiteId: number
        name: string
        bankName: string | null
        iban: string | null
        bic: string | null
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<BankRequisiteUpdateResult>(
        'crm.company.requisite.bankrequisite.update',
        {
          id: 23,
          fields: {
            comments: 'For supplier payments',
            bankAddress: 'Kurfürstendamm 21, 10719 Berlin, Germany',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bank requisite updated:', result.item)
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
      async function updateBankRequisite() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.company.requisite.bankrequisite.update',
            {
              id: 23,
              fields: {
                comments: 'For supplier payments',
                bankAddress: 'Kurfürstendamm 21, 10719 Berlin, Germany',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bank requisite updated:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateBankRequisite)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.company.requisite.bankrequisite.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 23,
                "fields": {
                    "comments": "For supplier payments",
                    "bankAddress": "Kurfürstendamm 21, 10719 Berlin, Germany",
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
                'crm.company.requisite.bankrequisite.update',
                [
                    'id' => 23,
                    'fields' => [
                        'comments' => 'For supplier payments',
                        'bankAddress' => 'Kurfürstendamm 21, 10719 Berlin, Germany',
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
        'crm.company.requisite.bankrequisite.update',
        {
            id: 23,
            fields: {
                comments: 'For supplier payments',
                bankAddress: 'Kurfürstendamm 21, 10719 Berlin, Germany',
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
        'crm.company.requisite.bankrequisite.update',
        [
            'id' => 23,
            'fields' => [
                'comments' => 'For supplier payments',
                'bankAddress' => 'Kurfürstendamm 21, 10719 Berlin, Germany',
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
    res, err := client.Core().Call(ctx, "crm.company.requisite.bankrequisite.update", b24.Params{
    	"id": 23,
    	"fields": b24.Params{
    		"comments":    "For supplier payments",
    		"bankAddress": "Kurfürstendamm 21, 10719 Berlin, Germany",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.company.requisite.bankrequisite.update: %w", err)
    }

    var result struct {
    	Item struct {
    		ID          int     `json:"id"`
    		RequisiteID int     `json:"requisiteId"`
    		Name        string  `json:"name"`
    		BankName    *string `json:"bankName"`
    		Iban        *string `json:"iban"`
    		Bic         *string `json:"bic"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 23,
            "requisiteId": 549,
            "name": "Main account",
            "comments": "For supplier payments",
            "bankName": "Berliner Handelsbank AG",
            "bankAddress": "Kurfürstendamm 21, 10719 Berlin, Germany",
            "bankRoutingNumber": "37040044",
            "bankAccountHolderName": "Müller GmbH",
            "bankAccountNumber": "0532013000",
            "iban": "DE89370400440532013000",
            "swift": null,
            "bic": "COBADEFFXXX"
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
[`object`](../../../data-types.md) | The bank detail after saving, with all fields available for the country of the requisite template. Empty fields are returned as `null`. [Field descriptions](./index.md#bank-fields) ||
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
                "message": "A bank requisite cannot be moved to another requisite.",
                "field": "requisiteId"
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
|| `id` | Required field `id` is missing | Pass the bank detail `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `fields` | The fields property must not be empty | Pass at least one field in `fields` ||
|| `requisiteId` | A bank requisite cannot be moved to another requisite | Remove `requisiteId` from `fields`. To move an account, delete the bank detail and add it to the target requisite ||
|| `bankAccountCurrency` | The bank requisite field is not available in the current template | The field is not in the set of bank fields for the country of the requisite template, for example, `bankAccountCurrency` in the German template. Pass fields from the set returned by the read methods ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field value must not be empty | Pass a non-empty `name` ||
|| `bic` | String must not be longer than 11 characters | Shorten the value to the specified length ||
|#

#### Bank Detail Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The bank detail does not exist, belongs to an owner of a different type, or you do not have permission to edit the owner. Check `id` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `BankRequisiteDto` | Check the field name against the [bank detail field descriptions](./index.md#bank-fields) ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.requisite.bankrequisite.update`. Requisite methods exist only for contacts and companies: the `company` or `contact` segment ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-requisite-bank-requisite-add-rest-v3.md)
- [{#T}](./crm-entity-requisite-bank-requisite-get-rest-v3.md)
- [{#T}](./crm-entity-requisite-bank-requisite-delete-rest-v3.md)
