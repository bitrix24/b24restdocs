# Get a CRM Form crm.form.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to view CRM forms

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `crm.form.get` returns a CRM form by identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Form identifier.

The identifier can be retrieved with [crm.form.list](./crm-form-list.md) ||
|| **select**
[`array`](../../data-types.md) | List of fields to return.

You can specify an entire section, for example `crm`, or a section field using dot notation, for example `crm.entityScheme`. All fields are described in [CRM Form Structure](./data-types.md#crm_form).

By default, the simple fields are returned: `id`, `code`, `name`, `active`, `system`, `languageId`, `templateId`, `createdTime`, `updatedTime`, `activeChangedById`, `activeChangedTime`.

The `appearance`, `crm`, `fields`, `dependencies`, `presetFields`, `responsible`, `agreements`, `submission`, `payment`, `callback`, `booking`, `analytics`, `spamProtection`, `integrations` sections are returned only if specified in `select` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"select":["id","name","active","updatedTime","crm","fields"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"select":["id","name","active","updatedTime","crm","fields"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormGetResult = {
      item: {
        id: number
        name: string
        active: boolean
        updatedTime: string
        crm: {
          entityScheme: number
          duplicateMode: string | null
        }
        fields: Array<{
          name: string
          type: string
          label: string | null
          required: boolean
        }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FormGetResult>(
        'crm.form.get',
        {
          id: 45,
          select: ['id', 'name', 'active', 'updatedTime', 'crm', 'fields'],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getForm() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.form.get',
            {
              id: 45,
              select: ['id', 'name', 'active', 'updatedTime', 'crm', 'fields'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getForm)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.form.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 45,
                "select": ["id", "name", "active", "updatedTime", "crm", "fields"],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.get',
                [
                    'id' => 45,
                    'select' => ['id', 'name', 'active', 'updatedTime', 'crm', 'fields'],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.get',
        {
          id: 45,
          select: ['id', 'name', 'active', 'updatedTime', 'crm', 'fields'],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.form.get',
        [
            'id' => 45,
            'select' => ['id', 'name', 'active', 'updatedTime', 'crm', 'fields'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.get", b24.Params{
    	"id": 45,
    	"select": []string{"id", "name", "active", "updatedTime", "crm", "fields"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var form struct {
    	ID          int    `json:"id"`
    	Name        string `json:"name"`
    	Active      bool   `json:"active"`
    	UpdatedTime string `json:"updatedTime"`
    }
    if err := json.Unmarshal(raw, &form); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(form.ID, form.Name, form.Active)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 45,
            "name": "Contact information",
            "active": true,
            "updatedTime": "2026-03-11T16:47:17+02:00",
            "crm": {
                "entityScheme": 1,
                "duplicateMode": "MERGE",
                "dealCategoryId": null,
                "dealDuplicateControlEnabled": true,
                "smartProcessCategoryId": null,
                "smartProcessDuplicateControlEnabled": false
            },
            "fields": [
                {
                    "name": "LEAD_NAME",
                    "type": "string",
                    "label": "Name",
                    "sort": 100,
                    "required": false,
                    "multiple": false,
                    "placeholder": "",
                    "valueType": null,
                    "value": null
                },
                {
                    "name": "LEAD_PHONE",
                    "type": "phone",
                    "label": "Phone",
                    "sort": 300,
                    "required": false,
                    "multiple": false,
                    "placeholder": "",
                    "valueType": null,
                    "value": null
                }
            ]
        }
    },
    "time": {
        "start": 1792593811,
        "finish": 1792593811.215759,
        "duration": 0.2157590389251709,
        "processing": 0,
        "date_start": "2026-10-21T17:43:31+02:00",
        "date_finish": "2026-10-21T17:43:31+02:00",
        "operating_reset_at": 1792594380,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **item**
[`object`](../../data-types.md) | Object with the form fields. The set of fields depends on `select`. [CRM Form Structure](./data-types.md#crm_form) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the form identifier ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `FormDto` | Pass fields from [CRM Form Structure](./data-types.md#crm_form) ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `abc` | Pass `select` as an array of strings, for example `["id","crm"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the permission to view CRM forms ||
|#

#### Form Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier with [crm.form.list](./crm-form-list.md) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-list.md)
- [{#T}](./crm-form-update.md)
- [{#T}](./data-types.md)
- [{#T}](./placement/crm-form-embedding-get.md)
- [{#T}](./index.md)
