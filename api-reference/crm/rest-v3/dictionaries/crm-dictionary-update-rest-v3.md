# Update a CRM Directory Value v 3.0 crm.dictionary.[directory].update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.dictionary.<directory>.update` method updates the name and sort order of a CRM directory value. The directory is set by the `<directory>` segment of the method name.

## Method Name for the Directory

The method is available only for [CRM value directories](./crm-dictionary-list-rest-v3.md#crm-values).

#|
|| **Directory** | **Method** ||
|| Deal types | `crm.dictionary.dealtype.update` ||
|| Contact types | `crm.dictionary.contacttype.update` ||
|| Company types | `crm.dictionary.companytype.update` ||
|| Salutations | `crm.dictionary.honorific.update` ||
|| Number of employees | `crm.dictionary.employees.update` ||
|| Industries | `crm.dictionary.industry.update` ||
|| Sources | `crm.dictionary.source.update` ||
|#

The method name is written in lowercase: `crm.dictionary.dealtype.update`, not `crm.dictionary.dealType.update`. If the name is written in a different case or contains a directory whose values cannot be changed, for example, `crm.dictionary.entitytype.update`, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Value ID, for example, `SUBSCRIPTION`. IDs are returned by the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields to update in the format `{"field": "value"}`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | New value name: a non-empty string of up to 100 characters. Bitrix24 removes leading and trailing spaces ||
|| **sort**
[`integer`](../../../data-types.md) | New sort order. Bitrix24 replaces zero and negative values with `10` ||
|#

The method changes only the passed fields. If `fields` is an empty object and a value with this `id` exists, the method returns `true` and changes nothing. The `id` and `isSystem` fields cannot be changed: the method returns an [object validation error](#dto-validation).

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.dictionary.dealtype.update`

{% endnote %}

The examples rename the `SUBSCRIPTION` deal type to "Annual subscription" and change its sort order. For a different directory, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"SUBSCRIPTION","fields":{"name":"Annual subscription","sort":65}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.dictionary.dealtype.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"SUBSCRIPTION","fields":{"name":"Annual subscription","sort":65},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.dictionary.dealtype.update
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DictionaryUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<DictionaryUpdateResult>(
        'crm.dictionary.dealtype.update',
        {
          id: 'SUBSCRIPTION',
          fields: {
            name: 'Annual subscription',
            sort: 65,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deal type updated:', result.result)
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
      async function updateDealType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.dictionary.dealtype.update',
            {
              id: 'SUBSCRIPTION',
              fields: {
                name: 'Annual subscription',
                sort: 65,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deal type updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDealType)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.dictionary.dealtype.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": "SUBSCRIPTION",
                "fields": {
                    "name": "Annual subscription",
                    "sort": 65,
                },
            },
            prefer_version=3,
        )
        print(response["result"]["result"])
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
                'crm.dictionary.dealtype.update',
                [
                    'id' => 'SUBSCRIPTION',
                    'fields' => [
                        'name' => 'Annual subscription',
                        'sort' => 65,
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
        'crm.dictionary.dealtype.update',
        {
            id: 'SUBSCRIPTION',
            fields: {
                name: 'Annual subscription',
                sort: 65,
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
        'crm.dictionary.dealtype.update',
        [
            'id' => 'SUBSCRIPTION',
            'fields' => [
                'name' => 'Annual subscription',
                'sort' => 65,
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
    res, err := client.Core().Call(ctx, "crm.dictionary.dealtype.update", b24.Params{
    	"id": "SUBSCRIPTION",
    	"fields": b24.Params{
    		"name": "Annual subscription",
    		"sort": 65,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.dictionary.dealtype.update: %w", err)
    }

    var updated struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &updated); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(updated.Result)
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
        "start": 1793715698,
        "finish": 1793715698.803068,
        "duration": 0.8030679225921631,
        "processing": 0,
        "date_start": "2026-11-03T17:21:38+03:00",
        "date_finish": "2026-11-03T17:21:38+03:00",
        "operating_reset_at": 1793716260,
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
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the request is completed. The new field values are returned by the [crm.dictionary.&lt;directory&gt;.get](./crm-dictionary-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Field \"id\" is read-only.",
                "field": "id"
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
|| `id` | Required field `id` is missing | Pass `id` ||
|| `id` | Field value must not be empty | Pass a non-empty ID ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `name` | Invalid field value. Check the value type and length | Pass a non-empty name of up to 100 characters ||
|| `sort` | This request requires data type `int` in field `sort` | Pass a value of the type specified in the field description ||
|#

#### Object Validation Errors {#dto-validation}

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Field `id` is read-only | Remove `id` from `fields`. To change the ID, create a new value with the [crm.dictionary.&lt;directory&gt;.add](./crm-dictionary-add-rest-v3.md) method ||
|| `isSystem` | Field `isSystem` is read-only | Remove `isSystem` from `fields` ||
|#

#### Value Not Found {#not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check the ID with the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) method ||
|#

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Remove `filter`: the method updates a single value by `id` and does not accept a filter ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `DictionaryValueDto` | Pass only `name` and `sort` in `fields`. An error with the field `0` means that `fields` was passed as a string rather than an object ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.dictionary.dealtype.update`. You can update values only in the directories from the table at the beginning of the page ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-dictionary-add-rest-v3.md)
- [{#T}](./crm-dictionary-get-rest-v3.md)
- [{#T}](./crm-dictionary-list-rest-v3.md)
- [{#T}](./crm-dictionary-delete-rest-v3.md)
