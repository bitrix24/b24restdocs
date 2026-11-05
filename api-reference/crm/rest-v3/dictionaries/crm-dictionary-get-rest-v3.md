# Get a CRM Directory Value v 3.0 crm.dictionary.[directory].get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view leads, deals, or other CRM objects

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.dictionary.<directory>.get` method returns a single CRM directory value by ID. The directory is set by the `<directory>` segment of the method name.

## Method Name for the Directory

#|
|| **Directory** | **Method** ||
|| Deal types | `crm.dictionary.dealtype.get` ||
|| Contact types | `crm.dictionary.contacttype.get` ||
|| Company types | `crm.dictionary.companytype.get` ||
|| Salutations | `crm.dictionary.honorific.get` ||
|| Number of employees | `crm.dictionary.employees.get` ||
|| Industries | `crm.dictionary.industry.get` ||
|| Sources | `crm.dictionary.source.get` ||
|| Phone types | `crm.dictionary.phonetype.get` ||
|| Email types | `crm.dictionary.emailtype.get` ||
|| Website types | `crm.dictionary.webtype.get` ||
|| Messenger types | `crm.dictionary.imtype.get` ||
|| Address types | `crm.dictionary.addresstype.get` ||
|| CRM object types | `crm.dictionary.entitytype.get` ||
|| Stage groups | `crm.dictionary.stagesemantic.get` ||
|| Activity providers | `crm.dictionary.activityprovider.get` ||
|#

The method name is written in lowercase: `crm.dictionary.dealtype.get`, not `crm.dictionary.dealType.get`. If the name is written in a different case or contains an unknown directory, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[```string | integer```](../../../data-types.md) | Value ID, for example, `SALE`. In the `addresstype` and `entitytype` directories, the ID is a number, for example, `6`.

IDs are returned by the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return. Each directory has its own set of fields, which is described on the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md#fields) page.

If the parameter is not passed or an empty array is passed, the method returns all fields ||
|#

In [CRM value directories](./crm-dictionary-list-rest-v3.md#crm-values), that is, the directories whose values can be changed, for example, `dealtype` or `source`, the ID is case-insensitive: `sale` finds `SALE`. In the other directories, pass the ID in the same case as in the response of the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) method.

The `entitytype` directory contains no SPAs. For an SPA type ID, for example, `1038`, the `crm.dictionary.entitytype.get` method returns the ["Value Not Found"](#not-found) error. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.dictionary.dealtype.get`

{% endnote %}

The examples retrieve the name of the `SALE` deal type and the default value flag. For a different directory, replace the method name according to the table above, take `id` from the response of the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) method, and check the fields in `select` against the [directory field descriptions](./crm-dictionary-list-rest-v3.md#fields).

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"SALE","select":["id","name","isSystem"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.dictionary.dealtype.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"SALE","select":["id","name","isSystem"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.dictionary.dealtype.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DictionaryGetResult = {
      item: {
        id: string
        name: string
        isSystem: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<DictionaryGetResult>(
        'crm.dictionary.dealtype.get',
        {
          id: 'SALE',
          select: ['id', 'name', 'isSystem'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deal type:', result.item.name)
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
      async function getDealType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.dictionary.dealtype.get',
            {
              id: 'SALE',
              select: ['id', 'name', 'isSystem'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deal type:', result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDealType)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.dictionary.dealtype.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": "SALE",
                "select": ["id", "name", "isSystem"],
            },
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["id"], item["name"], item["isSystem"])
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
                'crm.dictionary.dealtype.get',
                [
                    'id' => 'SALE',
                    'select' => ['id', 'name', 'isSystem'],
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
        'crm.dictionary.dealtype.get',
        {
            id: 'SALE',
            select: ['id', 'name', 'isSystem'],
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
        'crm.dictionary.dealtype.get',
        [
            'id' => 'SALE',
            'select' => ['id', 'name', 'isSystem'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.dictionary.dealtype.get", b24.Params{
    	"id":     "SALE",
    	"select": []string{"id", "name", "isSystem"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.dictionary.dealtype.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("response has no item key")
    }

    var item struct {
    	ID       string `json:"id"`
    	Name     string `json:"name"`
    	IsSystem bool   `json:"isSystem"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name, item.IsSystem)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": "SALE",
            "name": "Sales",
            "isSystem": true
        }
    },
    "time": {
        "start": 1793715633,
        "finish": 1793715633.516648,
        "duration": 0.5166480541229248,
        "processing": 0,
        "date_start": "2026-11-03T17:20:33+03:00",
        "date_finish": "2026-11-03T17:20:33+03:00",
        "operating_reset_at": 1793716200,
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
[`object`](../../../data-types.md) | Directory value. The set of fields is defined by the `select` parameter. [Field descriptions](./crm-dictionary-list-rest-v3.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID SUBSCRIPTION was not found."
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
|| `id` | This request requires data type `supported CRM Item entity type ID` in field `id` | An error of the `entitytype` directory: the number is not a CRM object type, for example, `999`. Take the ID from the [crm.dictionary.entitytype.list](./crm-dictionary-list-rest-v3.md) response ||
|#

#### Value Not Found {#not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check the ID with the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) method. The value may belong to a different directory: `CALL` is a source, not a deal type ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name in `select` against the [directory field descriptions](./crm-dictionary-list-rest-v3.md#fields) ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a user with permission to view leads, deals, or other CRM objects ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.dictionary.dealtype.get`. Take the directory name from the table at the beginning of the page ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-dictionary-add-rest-v3.md)
- [{#T}](./crm-dictionary-update-rest-v3.md)
- [{#T}](./crm-dictionary-list-rest-v3.md)
- [{#T}](./crm-dictionary-delete-rest-v3.md)
