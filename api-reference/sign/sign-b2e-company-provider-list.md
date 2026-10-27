# Get the List of Providers for sign.b2e.company.provider.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md), [`crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to create e-Signature for HR documents

The method `sign.b2e.company.provider.list` returns a list of signature providers for the selected company.

The method works only in the context of application authorization [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **companyUuid*** 
[`string`](../data-types.md) | UUID of the company in HCM Link.

Required if the `companyCrmId` parameter is not provided. You can retrieve the UUID using the [humanresources.hcmlink.company.list](./hcm-link/humanresources-hcmlink-company-list.md) method.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **companyCrmId*** 
[`integer`](../data-types.md) | Identifier of the company in CRM, connected in the integration as "my company".

Required if the `companyUuid` parameter is not provided. You can retrieve the identifier using the [crm.item.list](../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter and the `isMyCompany = Y` filter ||
|| **language** 
[`string`](../data-types.md) | Language of provider names. A Bitrix24 interface language code, for example `ru` or `en`.

If the language is not supported, names are returned in English. Defaults to `en` ||
|| **limit** 
[`integer`](../data-types.md) | Number of records per page.

The parameter accepts values from 1 to 1000.

Defaults to 100 ||
|| **offset** 
[`integer`](../data-types.md) | Number of providers to skip from the beginning of the list. To retrieve the next page, increase `offset` by the `limit` value.

Defaults to 0 ||
|#

You must provide either the `companyUuid` or `companyCrmId` parameter.

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"companyCrmId":12,"limit":2,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.company.provider.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each provider item returned in result[]
    type ProviderItem = {
      code: string
      uid: string
      name: string
      date: ISODate | null
      expires: ISODate | null
    }

    try {
      // Returns up to `limit` providers (1-1000, default 100) starting from `offset`.
      // To get the next page, repeat the call with offset increased by limit.
      const response = await $b24.actions.v2.call.make<ProviderItem[]>({
        method: 'sign.b2e.company.provider.list',
        params: {
          // UUID of the company in HCM Link or its CRM company ID
          companyCrmId: 12,
          // Language for provider name localization
          language: 'en',
          // Pagination parameters
          limit: 2,
          offset: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.length, result[0]?.name, result[0]?.uid)
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
      async function fetchCompanyProviders() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // Returns up to `limit` providers (1-1000, default 100) starting from `offset`.
          // To get the next page, repeat the call with offset increased by limit.
          const response = await $b24.actions.v2.call.make({
            method: 'sign.b2e.company.provider.list',
            params: {
              // UUID of the company in HCM Link or its CRM company ID
              companyCrmId: 12,
              // Language for provider name localization
              language: 'en',
              // Pagination parameters
              limit: 2,
              offset: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.length, result[0]?.name, result[0]?.uid)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchCompanyProviders)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sign.b2e.company.provider.list(
            company_crm_id=12,
            language="en",
            limit=2,
            offset=0,
        ).response
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.company.provider.list',
                [
                    'companyCrmId' => 12,
                    'language' => 'de',
                    'limit' => 2,
                    'offset' => 0
                ]
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

    ```javascript
    BX24.callMethod(
        'sign.b2e.company.provider.list',
        {
            companyCrmId: 12,
            language: 'de',
            limit: 2,
            offset: 0
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.company.provider.list',
        [
            'companyCrmId' => 12,
            'language' => 'de',
            'limit' => 2,
            'offset' => 0
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.company.provider.list", b24.Params{
    	"companyCrmId": 12,
    	"limit":        2,
    	"offset":       0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.company.provider.list: %w", err)
    }

    var items []struct {
    	Code    string `json:"code"`
    	Uid     string `json:"uid"`
    	Name    string `json:"name"`
    	Date    string `json:"date"`
    	Expires string `json:"expires"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Code, it.Uid)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "code": "ses-de",
            "uid": "d4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b",
            "name": "Bitrix24 e-Signature for HR",
            "date": "2025-02-18T10:15:30+01:00",
            "expires": "2026-02-18T10:15:30+01:00"
        }
    ],
    "time": {
        "start": 1739860000.123,
        "finish": 1739860000.456,
        "duration": 0.333,
        "processing": 0.111,
        "date_start": "2025-02-18T09:19:34+01:00",
        "date_finish": "2025-02-18T09:19:34+01:00",
        "operating_reset_at": 1739860600,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | List of signature providers for the company ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result array

#| 
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Provider code ||
|| **uid**
[`string`](../data-types.md) | Unique identifier of the provider ||
|| **name**
[`string`](../data-types.md) | Name of the provider considering the language ||
|| **date**
[`string`](../data-types.md) | Registration date of the provider in ISO 8601 format ||
|| **expires**
[`string`](../data-types.md) | Expiration date of the provider in ISO 8601 format ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "510",
    "error_description": "Company was not found."
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! User authorization required | The request was made without user authorization ||
|| `403` | `ACCESS_DENIED` | Access denied! | e-Signature for HR is not available on Bitrix24, the application lacks the `sign.b2e` or `crm` scope, or the user does not have permission to create documents in e-Signature for HR. Also occurs if `companyUuid` is passed but the `humanresources.hcmlink` scope is not connected ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was not called from an application with OAuth authorization, for example, through a webhook ||
|| `400` | Empty value | Parameter 'companyUuid' or 'companyCrmId' is required | Neither `companyUuid` nor `companyCrmId` was passed ||
|| `400` | `510` | Company was not found. | The company was not found by `companyUuid` or `companyCrmId`. The method also returns this error if a non-identifier value, such as an array, is passed in `companyCrmId` ||
|| `400` | `0` | Error while autoregistering providers | Failed to register the company's providers in the signing service. Check the company details in CRM: the company must have a tax ID ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-document-send.md)
- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./sign-b2e-provider-tail.md)
- [{#T}](./index.md)
