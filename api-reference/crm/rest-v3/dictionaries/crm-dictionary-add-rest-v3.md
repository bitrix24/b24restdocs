# Add a CRM Directory Value v 3.0 crm.dictionary.[directory].add

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

The `crm.dictionary.<directory>.add` method adds a value to a CRM directory and returns the whole value. The directory is set by the `<directory>` segment of the method name.

## Method Name for the Directory

The method is available only for [CRM value directories](./crm-dictionary-list-rest-v3.md#crm-values).

#|
|| **Directory** | **Method** ||
|| Deal types | `crm.dictionary.dealtype.add` ||
|| Contact types | `crm.dictionary.contacttype.add` ||
|| Company types | `crm.dictionary.companytype.add` ||
|| Salutations | `crm.dictionary.honorific.add` ||
|| Number of employees | `crm.dictionary.employees.add` ||
|| Industries | `crm.dictionary.industry.add` ||
|| Sources | `crm.dictionary.source.add` ||
|#

The method name is written in lowercase: `crm.dictionary.dealtype.add`, not `crm.dictionary.dealType.add`. If the name is written in a different case or contains a directory whose values cannot be changed, for example, `crm.dictionary.entitytype.add`, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new value in the format `{"field": "value"}`. [Parameter description](#fields)

Required field — `name` ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Value ID, for example, `SUBSCRIPTION`. It cannot be changed after the value is created.

Up to 50 characters, without a colon, not `0`, and not whitespace only. If the field is not passed or an empty string is passed, Bitrix24 generates an ID such as `UC_1Z8DXI` ||
|| **name***
[`string`](../../../data-types.md) | Value name, up to 100 characters. Bitrix24 removes leading and trailing spaces ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order. Bitrix24 replaces zero and negative values with `10`.

Default: `10` ||
|#

The `isSystem` field is read-only: the method does not accept it in `fields`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.dictionary.dealtype.add`

{% endnote %}

The examples add the "Subscription" deal type with the ID `SUBSCRIPTION`. For a different directory, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"id":"SUBSCRIPTION","name":"Subscription","sort":60}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.dictionary.dealtype.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"id":"SUBSCRIPTION","name":"Subscription","sort":60},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.dictionary.dealtype.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DictionaryAddResult = {
      item: {
        id: string
        name: string
        sort: number
        isSystem: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<DictionaryAddResult>(
        'crm.dictionary.dealtype.add',
        {
          fields: {
            id: 'SUBSCRIPTION',
            name: 'Subscription',
            sort: 60,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deal type created:', result.item.id)
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
      async function addDealType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.dictionary.dealtype.add',
            {
              fields: {
                id: 'SUBSCRIPTION',
                name: 'Subscription',
                sort: 60,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deal type created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDealType)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.dictionary.dealtype.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "id": "SUBSCRIPTION",
                    "name": "Subscription",
                    "sort": 60,
                },
            },
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["id"], item["name"])
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
                'crm.dictionary.dealtype.add',
                [
                    'fields' => [
                        'id' => 'SUBSCRIPTION',
                        'name' => 'Subscription',
                        'sort' => 60,
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
        'crm.dictionary.dealtype.add',
        {
            fields: {
                id: 'SUBSCRIPTION',
                name: 'Subscription',
                sort: 60,
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
        'crm.dictionary.dealtype.add',
        [
            'fields' => [
                'id' => 'SUBSCRIPTION',
                'name' => 'Subscription',
                'sort' => 60,
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
    res, err := client.Core().Call(ctx, "crm.dictionary.dealtype.add", b24.Params{
    	"fields": b24.Params{
    		"id":   "SUBSCRIPTION",
    		"name": "Subscription",
    		"sort": 60,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.dictionary.dealtype.add: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("response has no item key")
    }

    var item struct {
    	ID   string `json:"id"`
    	Name string `json:"name"`
    	Sort int    `json:"sort"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": "SUBSCRIPTION",
            "name": "Subscription",
            "sort": 60,
            "isSystem": false
        }
    },
    "time": {
        "start": 1793715698,
        "finish": 1793715698.298801,
        "duration": 0.2988009452819824,
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
|| **result.item**
[`object`](../../../data-types.md) | The created value with all fields: `id`, `name`, `sort`, `isSystem`. If `id` or `sort` was not passed, the response contains the values assigned by Bitrix24. Pass the `id` in a CRM item field, for example, in the `typeId` field of a deal. [Field descriptions](./crm-dictionary-list-rest-v3.md#crm-values) ||
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
                "message": "This ID is already in use. Try a different one.",
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
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `id` | This ID is already in use. Try a different one | Pass a different `id`, or do not pass it so that Bitrix24 generates an ID ||
|| `id` | The ID must not exceed 50 characters. Colon characters are not allowed. It must not resolve to `0` | Pass an ID of up to 50 characters without a colon and other than `0` ||
|| `name` | Invalid field value. Check the value type and length | Pass a non-empty name of up to 100 characters ||
|| `sort` | This request requires data type `int` in field `sort` | Pass a value of the type specified in the field description ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `isSystem` | Field `isSystem` is read-only | Remove `isSystem` from `fields` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `DictionaryValueDto` | Pass only `id`, `name`, and `sort` in `fields`. An error with the field `0` means that `fields` was passed as a string rather than an object ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.dictionary.dealtype.add`. You can add values only to the directories from the table at the beginning of the page ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-dictionary-update-rest-v3.md)
- [{#T}](./crm-dictionary-get-rest-v3.md)
- [{#T}](./crm-dictionary-list-rest-v3.md)
- [{#T}](./crm-dictionary-delete-rest-v3.md)
