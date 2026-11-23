# Get an SPA Type Field Description v 3.0 crm.smartprocess.field.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.smartprocess.field.get` method returns the description of a single SPA type field by the field name.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field name in camelCase, for example, `daysBeforeClose`. The name is case-sensitive: the method does not find `DaysBeforeClose`.

Field names are returned by the [crm.smartprocess.field.list](./crm-smart-process-field-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of description fields to return, for example, `["name", "editable"]`. [Description fields](./crm-smart-process-field-list-rest-v3.md#item).

By default, the method returns all description fields ||
|#

The method describes only top-level fields. It does not find nested fields, for example, `relations.parent`, and returns the ["Field Not Found"](#field-not-found) error.

The method name is written in lowercase: `crm.smartprocess.field.get`. For the name `crm.smartProcess.field.get`, the method returns the ["Method Not Found"](#method-not-found) error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.field.get`

{% endnote %}

The examples retrieve the description of the `daysBeforeClose` field, the item completion deadline.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"daysBeforeClose"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"daysBeforeClose","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.field.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldGetResult = {
      item: {
        name: string
        type: string
        filterable: boolean
        sortable: boolean
        editable: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<FieldGetResult>(
        'crm.smartprocess.field.get',
        {
          name: 'daysBeforeClose',
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const item = response.getData()!.result.item
        console.info('Field:', item.name, item.type, item.editable)
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
      async function getSmartProcessTypeField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.field.get',
            {
              name: 'daysBeforeClose',
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const item = response.getData().result.item
          console.info('Field:', item.name, item.type, item.editable)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSmartProcessTypeField)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.field.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={"name": "daysBeforeClose"},
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["name"], item["type"], item["editable"])
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
                'crm.smartprocess.field.get',
                [
                    'name' => 'daysBeforeClose',
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
        'crm.smartprocess.field.get',
        {
            name: 'daysBeforeClose',
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
        'crm.smartprocess.field.get',
        [
            'name' => 'daysBeforeClose',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess.field.get", b24.Params{
    	"name": "daysBeforeClose",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.field.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("response has no item key")
    }

    var item struct {
    	Name       string `json:"name"`
    	Type       string `json:"type"`
    	Filterable bool   `json:"filterable"`
    	Sortable   bool   `json:"sortable"`
    	Editable   bool   `json:"editable"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Name, item.Type, item.Editable)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "name": "daysBeforeClose",
            "type": "int",
            "title": "daysBeforeClose",
            "description": null,
            "validationRules": [],
            "requiredGroups": null,
            "filterable": true,
            "sortable": true,
            "editable": true,
            "editableGroups": [],
            "multiple": false,
            "elementType": null
        }
    },
    "time": {
        "start": 1795431317,
        "finish": 1795431317.997235,
        "duration": 0.9972350597381592,
        "processing": 0,
        "date_start": "2026-11-23T11:55:17+01:00",
        "date_finish": "2026-11-23T11:55:17+01:00",
        "operating_reset_at": 1795431900,
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
[`object`](./crm-smart-process-field-list-rest-v3.md#item) | Field description. The set of fields is defined by the `select` parameter. [Description fields](./crm-smart-process-field-list-rest-v3.md#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"Title\" was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Field Not Found {#field-not-found}

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `Title` was not found | Check the name and case against the [crm.smartprocess.field.list](./crm-smart-process-field-list-rest-v3.md) response. The method does not describe dot-notation nested fields, for example, `relations.parent` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the field name in `name` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`, HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `DtoFieldDto` | Check the names in `select` against the [description fields table](./crm-smart-process-field-list-rest-v3.md#item) ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.field.get` was not found | Write the method name in lowercase: `crm.smartprocess.field.get` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-field-list-rest-v3.md)
- [{#T}](./crm-smart-process-add-rest-v3.md)
- [{#T}](./crm-smart-process-update-rest-v3.md)
