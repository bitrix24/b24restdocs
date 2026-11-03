# Get a Requisite Template v 3.0 crm.[type].requisite.preset.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view contacts or companies

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.requisite.preset.get` method returns a requisite template by ID together with its fields. The fields show what can be filled in for a requisite with this template. The owner type in the method name affects only the `isDefault` flag.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Company | `crm.company.requisite.preset.get` ||
|| Contact | `crm.contact.requisite.preset.get` ||
|#

Template methods only read data. Templates are created and configured in the Bitrix24 interface.

The method name is written in lowercase: `crm.company.requisite.preset.get`, not `crm.company.requisite.Preset.Get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Template ID.

The ID can be retrieved with the [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method. The method does not return an inactive template ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.company.requisite.preset.get`

{% endnote %}

The examples retrieve the German "Unternehmen" template with the ID `1`. On your Bitrix24, the template ID may differ: retrieve it with the [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method. For a contact, replace the method name with `crm.contact.requisite.preset.get`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.requisite.preset.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.company.requisite.preset.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PresetField = {
      name: string
      kind: 'requisiteField' | 'address' | 'userField'
      title: string
      inShortList: boolean
      sort: number
    }

    type PresetGetResult = {
      item: {
        id: number
        name: string
        isDefault: boolean
        sort: number
        fields: PresetField[]
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<PresetGetResult>(
        'crm.company.requisite.preset.get',
        {
          id: 1,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Preset:', result.item)
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
      async function getPreset() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.company.requisite.preset.get',
            {
              id: 1,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Preset:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPreset)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.company.requisite.preset.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 1,
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
                'crm.company.requisite.preset.get',
                [
                    'id' => 1,
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
        'crm.company.requisite.preset.get',
        {
            id: 1,
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
        'crm.company.requisite.preset.get',
        [
            'id' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.company.requisite.preset.get", b24.Params{
    	"id": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.company.requisite.preset.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID        int    `json:"id"`
    		Name      string `json:"name"`
    		IsDefault bool   `json:"isDefault"`
    		Fields    []struct {
    			Name string `json:"name"`
    			Kind string `json:"kind"`
    		} `json:"fields"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, field := range result.Item.Fields {
    	fmt.Println(field.Kind, field.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 1,
            "name": "Unternehmen",
            "isDefault": true,
            "sort": 500,
            "fields": [
                {
                    "name": "companyTitle",
                    "kind": "requisiteField",
                    "title": "",
                    "inShortList": true,
                    "sort": 510
                },
                {
                    "name": "vatNumber",
                    "kind": "requisiteField",
                    "title": "",
                    "inShortList": false,
                    "sort": 520
                },
                {
                    "name": "usrle",
                    "kind": "requisiteField",
                    "title": "",
                    "inShortList": false,
                    "sort": 530
                },
                {
                    "name": "inn",
                    "kind": "requisiteField",
                    "title": "",
                    "inShortList": true,
                    "sort": 540
                },
                {
                    "name": "",
                    "kind": "address",
                    "title": "",
                    "inShortList": false,
                    "sort": 550
                }
            ]
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
|| **result.item.id**
[`integer`](../../../data-types.md) | Template ID. Passed in the `presetId` of a requisite ||
|| **result.item.name**
[`string`](../../../data-types.md) | Template name ||
|| **result.item.isDefault**
[`boolean`](../../../data-types.md) | `true` if the template is the default one for the owner type from the method name. The same template can be the default for companies and a regular one for contacts ||
|| **result.item.sort**
[`integer`](../../../data-types.md) | Template sort order ||
|| **result.item.fields**
[`object[]`](../../../data-types.md) | Template fields in the order they appear in the card. Each element contains:

- `name` — field name. For `kind: "requisiteField"`, the name of a requisite field that can be passed in `fields` of the `add` and `update` methods. For `kind: "address"`, an empty string
- `kind` — element kind: `requisiteField` is a requisite field, `address` means the template has addresses, `userField` is a custom field
- `title` — field title in the template. An empty string if the standard title is used
- `inShortList` — whether the field is shown in the short card form
- `sort` — field order in the template ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the template `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|#

#### Template Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | The template does not exist or is inactive. Retrieve the available templates with the [crm.&lt;type&gt;.requisite.preset.list](./crm-entity-requisite-preset-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `select` found in entity `PresetDto` | Remove `select` and `filter` from the request: the template is returned in full ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | You need permission to view contacts or companies, depending on the type in the method name ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.company.requisite.preset.get`. Requisite methods exist only for contacts and companies: the `company` or `contact` segment ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-requisite-preset-list-rest-v3.md)
- [{#T}](./crm-entity-requisite-add-rest-v3.md)
