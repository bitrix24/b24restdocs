# Get a List of Personal Details Fields sign.b2e.legal.field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the personal details section in the employee profile

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The `sign.b2e.legal.field.list` method returns the fields of the personal details section in the employee profile: full name, position, address, Social Security number, Taxpayer ID, and fields added by an HR specialist. e-Signature for HR populates documents with values from these fields.

For each field, the method returns the value format, whether the value can be written with the [sign.b2e.legal.profile.update](./sign-b2e-legal-profile-update.md) method, and how the field is referenced in a docx template. The fields are returned in the same order as in the field list of the template editor.

An HR specialist can add and delete fields at any time, so do not cache the list for long: request it before writing values.

The method works in an app and via an inbound webhook.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/sign.b2e.legal.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/sign.b2e.legal.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/sign.b2e.legal.field.list
    ```

- JS (TS)

    The REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type LegalFieldListResult = {
      items: Array<{
        code: string
        type: string
        caption: string
        writable: boolean
        signerPlaceholder: string | null
        assigneePlaceholder: string | null
        options: Array<{ id: number, value: string }> | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<LegalFieldListResult>(
        'sign.b2e.legal.field.list',
        {},
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items
          .filter(field => field.writable)
          .forEach(field => console.info(field.code, field.type, field.caption))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    The REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listLegalFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'sign.b2e.legal.field.list',
            {},
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items
            .filter(field => field.writable)
            .forEach(field => console.info(field.code, field.type, field.caption))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listLegalFields)
    </script>
    ```

- Python

    The SDK sends calls to REST 3.0 only for methods in the `Config().api_v3_methods` list, and the `sign.b2e.*` methods are not on it yet. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "sign.b2e.legal.field.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={},
            prefer_version=3,
        )
        for field in response["result"]["items"]:
            if field["writable"]:
                print(field["code"], field["type"], field["caption"])
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.legal.field.list',
                []
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'sign.b2e.legal.field.list',
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.legal.field.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.legal.field.list", b24.Params{}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.legal.field.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		Code     string `json:"code"`
    		Type     string `json:"type"`
    		Caption  string `json:"caption"`
    		Writable bool   `json:"writable"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, field := range result.Items {
    	if field.Writable {
    		fmt.Println(field.Code, field.Type, field.Caption)
    	}
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "code": "UF_LEGAL_NAME",
                "type": "string",
                "caption": "First name",
                "writable": true,
                "signerPlaceholder": "User.Emp.Name",
                "assigneePlaceholder": "User.Rep.Name",
                "options": null
            },
            {
                "code": "UF_LEGAL_LAST_NAME",
                "type": "string",
                "caption": "Last name",
                "writable": true,
                "signerPlaceholder": "User.Emp.LastName",
                "assigneePlaceholder": "User.Rep.LastName",
                "options": null
            },
            {
                "code": "UF_LEGAL_PATRONYMIC_NAME",
                "type": "string",
                "caption": "Second name",
                "writable": true,
                "signerPlaceholder": "User.Emp.Patronymic",
                "assigneePlaceholder": "User.Rep.Patronymic",
                "options": null
            },
            {
                "code": "UF_LEGAL_FULL_NAME",
                "type": "fullname",
                "caption": "Full name",
                "writable": false,
                "signerPlaceholder": "User.Emp.FullName",
                "assigneePlaceholder": "User.Rep.FullName",
                "options": null
            },
            {
                "code": "UF_LEGAL_POSITION",
                "type": "string",
                "caption": "Position",
                "writable": true,
                "signerPlaceholder": "User.Emp.Pos",
                "assigneePlaceholder": "User.Rep.Pos",
                "options": null
            },
            {
                "code": "UF_LEGAL_ADDRESS",
                "type": "string",
                "caption": "Address",
                "writable": true,
                "signerPlaceholder": "User.Emp.Addr",
                "assigneePlaceholder": "User.Rep.Addr",
                "options": null
            }
        ]
    },
    "time": {
        "start": 1794476870,
        "finish": 1794476870.412755,
        "duration": 0.41275501251220703,
        "processing": 0,
        "date_start": "2026-11-12T12:47:50+01:00",
        "date_finish": "2026-11-12T12:47:50+01:00",
        "operating_reset_at": 1794477420,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../data-types.md) | Array of fields [(detailed description)](#items) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Items Array Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Field code, for example `UF_LEGAL_POSITION`. Pass it as a key in the `fields` parameter of the [sign.b2e.legal.profile.update](./sign-b2e-legal-profile-update.md) method ||
|| **type**
[`string`](../data-types.md) | Value format.

Possible values:

- `string` — string
- `snils` — Social Security number
- `inn` — Taxpayer ID
- `date` — date
- `address` — address
- `url` — link
- `enumeration` — list. The value options are in `options`
- `fullname` — full name. It is composed of the first name, last name, and second name and cannot be written

If a different type is returned, treat the field value as not writable ||
|| **caption**
[`string`](../data-types.md) | Field name in the Bitrix24 language ||
|| **writable**
[`boolean`](../data-types.md) | Whether the value can be written with the `sign.b2e.legal.profile.update` method.

Possible values:

- `true` — it can
- `false` — it cannot, for example for a `fullname` field or a multiple field ||
|| **signerPlaceholder**
[`string`](../data-types.md) or `null` | Placeholder for a docx template: e-Signature for HR replaces it with the field value from the employee profile. `null` if the field cannot be used in templates ||
|| **assigneePlaceholder**
[`string`](../data-types.md) or `null` | Placeholder for a docx template: e-Signature for HR replaces it with the field value from the profile of the company representative. `null` if the field cannot be used in templates ||
|| **options**
[`array`](../data-types.md) or `null` | Value options for an `enumeration` field: an array of objects with the `id` and `value` keys. `null` for other types ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "SIGN_B2E_LEGAL_ACCESS_DENIED",
        "message": "Insufficient permissions to view or edit personal details in the employee profile. Please contact your Bitrix24 administrator."
    }
}
```

{% include notitle [Error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

HTTP status: **403**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_LEGAL_ACCESS_DENIED` | Insufficient permissions to view or edit personal details in the employee profile. Please contact your Bitrix24 administrator | Grant the user permission to view the personal details section in the employee profile. The method must be called by an active employee: a service account, an extranet user, and a collab guest receive this error ||
|| `SIGN_REST_AUTH_TYPE_NOT_ALLOWED` | This method is available to apps and inbound webhooks only | Call the method from an app or via an inbound webhook ||
|| `SIGN_B2E_UNAVAILABLE` | HR documents are not available in this Bitrix24 | Make sure e-Signature for HR is available in your Bitrix24 ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-legal-profile-update.md)
- [{#T}](./sign-b2e-template-list.md)
- [{#T}](./index.md)
