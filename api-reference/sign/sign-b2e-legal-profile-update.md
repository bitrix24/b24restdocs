# Store Personal Details in the Employee Profile sign.b2e.legal.profile.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`, `user`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the personal details section in the employee profile

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The `sign.b2e.legal.profile.update` method writes values of the personal details fields to a user profile, for example position, address, Social Security number, or Taxpayer ID. e-Signature for HR populates documents with values from these fields.

The method writes either all the passed values or none of them. If at least one value has an invalid format, the method returns an error listing such fields, and the profile is not changed.

You can write values to the profile of an active user — an employee or an extranet user. You cannot write them to the profile of a service account, such as a bot.

The method works in an app and via an inbound webhook. The app or webhook needs two scopes: `sign.b2e` and `user`.

The method can be called inside a [batch](../rest-v3.md#batch). The method ignores the `Idempotency-Key` header: a repeated call writes the same values again.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **userId***
[`integer`](../data-types.md) | Identifier of the user whose profile the values are written to.

You can retrieve the identifier with the [user.get](../user/user-get.md) method ||
|| **fields***
[`object`](../data-types.md) | Field values in the `{"field code": value}` format, from 1 to 50 fields.

Field codes are returned by the [sign.b2e.legal.field.list](./sign-b2e-legal-field-list.md) method. You can write only fields with `writable` set to `true`. The value format depends on the field type [(detailed description)](#fields) ||
|#

The method accepts no other parameters: a call with them returns a validation error.

### Field Values {#fields}

The field type is the `type` value in the [sign.b2e.legal.field.list](./sign-b2e-legal-field-list.md#items) response. The method trims leading and trailing spaces from the value. You cannot write an empty value or a value consisting only of spaces.

#|
|| **Field type** | **Value** ||
|| `string` | String of up to 2000 characters ||
|| `snils` | Social Security number with a valid checksum. It can be passed with spaces and hyphens ||
|| `inn` | Taxpayer ID with a valid checksum ||
|| `date` | Date in the `YYYY-MM-DD` format, for example `2026-11-12` ||
|| `url` | Link that starts with `http://` or `https://`, up to 2000 characters. The domain can contain non-ASCII characters, for example `https://müller-gmbh.de` ||
|| `address` | Address as a string or as an object with address parts. The string or each part can be up to 500 characters long and must not contain the `\|` character. A string is written to the `ADDRESS_2` part.

Object keys:

- `ADDRESS_1` — street and building
- `ADDRESS_2` — apartment, office
- `CITY` — city
- `POSTAL_CODE` — postal code
- `REGION` — district
- `PROVINCE` — region
- `COUNTRY` — country ||
|| `enumeration` | Identifier of an option from `options` as a number or a string of digits ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/sign.b2e.legal.profile.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":1595,"fields":{"UF_LEGAL_POSITION":"Sales manager","UF_LEGAL_LAST_NAME":"Weber"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/sign.b2e.legal.profile.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":1595,"fields":{"UF_LEGAL_POSITION":"Sales manager","UF_LEGAL_LAST_NAME":"Weber"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/sign.b2e.legal.profile.update
    ```

- JS (TS)

    The REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type LegalProfileUpdateResult = {
      item: {
        userId: number
        updated: string[]
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<LegalProfileUpdateResult>(
        'sign.b2e.legal.profile.update',
        {
          userId: 1595,
          fields: {
            UF_LEGAL_POSITION: 'Sales manager',
            UF_LEGAL_LAST_NAME: 'Weber',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated:', result.item.updated.join(', '))
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
      async function updateLegalProfile() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'sign.b2e.legal.profile.update',
            {
              userId: 1595,
              fields: {
                UF_LEGAL_POSITION: 'Sales manager',
                UF_LEGAL_LAST_NAME: 'Weber',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated:', result.item.updated.join(', '))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateLegalProfile)
    </script>
    ```

- Python

    The SDK sends calls to REST 3.0 only for methods in the `Config().api_v3_methods` list, and the `sign.b2e.*` methods are not on it yet. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "sign.b2e.legal.profile.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "userId": 1595,
                "fields": {
                    "UF_LEGAL_POSITION": "Sales manager",
                    "UF_LEGAL_LAST_NAME": "Weber",
                },
            },
            prefer_version=3,
        )
        print("Updated:", ", ".join(response["result"]["item"]["updated"]))
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
                'sign.b2e.legal.profile.update',
                [
                    'userId' => 1595,
                    'fields' => [
                        'UF_LEGAL_POSITION' => 'Sales manager',
                        'UF_LEGAL_LAST_NAME' => 'Weber',
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'sign.b2e.legal.profile.update',
        {
            userId: 1595,
            fields: {
                UF_LEGAL_POSITION: 'Sales manager',
                UF_LEGAL_LAST_NAME: 'Weber',
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

    The SDK does not yet support calls to the `/rest/api/` address. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.legal.profile.update',
        [
            'userId' => 1595,
            'fields' => [
                'UF_LEGAL_POSITION' => 'Sales manager',
                'UF_LEGAL_LAST_NAME' => 'Weber',
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
    res, err := client.Core().Call(ctx, "sign.b2e.legal.profile.update", b24.Params{
    	"userId": 1595,
    	"fields": b24.Params{
    		"UF_LEGAL_POSITION":  "Sales manager",
    		"UF_LEGAL_LAST_NAME": "Weber",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sign.b2e.legal.profile.update: %w", err)
    }

    var result struct {
    	Item struct {
    		UserID  int      `json:"userId"`
    		Updated []string `json:"updated"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.UserID, result.Item.Updated)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "userId": 1595,
            "updated": [
                "UF_LEGAL_POSITION",
                "UF_LEGAL_LAST_NAME"
            ]
        }
    },
    "time": {
        "start": 1794477046,
        "finish": 1794477046.481356,
        "duration": 0.4813559055328369,
        "processing": 0,
        "date_start": "2026-11-12T12:50:46+01:00",
        "date_finish": "2026-11-12T12:50:46+01:00",
        "operating_reset_at": 1794477600,
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
|| **result.item**
[`object`](../data-types.md) | Write result [(detailed description)](#item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`integer`](../data-types.md) | Identifier of the user whose profile the values were written to ||
|| **updated**
[`array`](../data-types.md) | Codes of the fields whose values were written. The method does not return the values themselves ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "SIGN_B2E_LEGAL_FIELDS_INVALID",
        "message": "Check the personal details fields you passed.",
        "validation": [
            {
                "field": "UF_LEGAL_NAME",
                "code": "SIGN_B2E_LEGAL_FIELD_EMPTY"
            },
            {
                "field": "UF_LEGAL_FULL_NAME",
                "code": "SIGN_B2E_LEGAL_FIELD_READ_ONLY"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Field Value Errors

Error Code: `SIGN_B2E_LEGAL_FIELDS_INVALID`

HTTP status: **400**

The `error.validation` array lists all fields that failed validation: `field` is the field code, and `code` is the reason code from the table. Field values are not included in the response.

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_LEGAL_FIELD_UNKNOWN` | No such field exists in Bitrix24 | Take the field code from the [sign.b2e.legal.field.list](./sign-b2e-legal-field-list.md) response ||
|| `SIGN_B2E_LEGAL_FIELD_READ_ONLY` | The field value cannot be written | Pass only fields with `writable` set to `true`. Write the full name in parts — to the first name, last name, and second name fields, for example `UF_LEGAL_NAME` ||
|| `SIGN_B2E_LEGAL_FIELD_EMPTY` | An empty value or `null` was passed | Pass a non-empty value ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_TYPE` | The value has the wrong type, for example a number instead of a string | Pass the value in the format from the [table](#fields) ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_SNILS` | Invalid Social Security number | Check the digits and the checksum ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_INN` | Invalid Taxpayer ID | Check the digits and the checksum ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_DATE` | The date is not in the `YYYY-MM-DD` format or does not exist | Pass a date, for example `2026-11-12` ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_URL` | Invalid link | Pass a link that starts with `http://` or `https://` ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_ADDRESS` | The address is longer than 500 characters, contains `\|`, or the object has an unknown key | Check the length, characters, and keys of the address parts ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_OPTION` | No such option in the list | Pass the `id` of an option from `options` ||
|| `SIGN_B2E_LEGAL_FIELD_INVALID_VALUE` | The value failed validation, for example a string longer than 2000 characters | Shorten or correct the value ||
|#

#### User Errors

HTTP status: **400**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_LEGAL_USER_NOT_FOUND` | The user with this ID was not found | Check `userId`. You can retrieve the identifier with the [user.get](../user/user-get.md) method ||
|| `SIGN_B2E_LEGAL_USER_INACTIVE` | Cannot specify personal details for a dismissed user | Specify an active user ||
|| `SIGN_B2E_LEGAL_USER_NOT_SUPPORTED` | Cannot specify personal details for a service account or collab guest | Specify a user who is not a service account ||
|#

#### Access Errors

HTTP status: **403**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_LEGAL_ACCESS_DENIED` | Insufficient permissions to view or edit personal details in the employee profile. Please contact your Bitrix24 administrator | Grant the user permission to edit the personal details section in the employee profile. The method must be called by an active employee: a service account, an extranet user, and a collab guest receive this error ||
|| `SIGN_REST_SCOPE_REQUIRED` | Missing access permission: user. Add it in the settings of your app or inbound webhook | Add the `user` scope to the app or webhook ||
|| `SIGN_REST_AUTH_TYPE_NOT_ALLOWED` | This method is available to apps and inbound webhooks only | Call the method from an app or via an inbound webhook ||
|| `SIGN_B2E_UNAVAILABLE` | HR documents are not available in this Bitrix24 | Make sure e-Signature for HR is available in your Bitrix24 ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `userId` | Required field `userId` is missing | Add `userId` to the request body ||
|| `fields` | Field value must not be empty | Pass at least one field ||
|| `fields` | Too many fields or invalid field code. Pass up to 50 fields using the codes returned by the sign.b2e.legal.field.list method | Pass up to 50 fields. A field code starts with `UF_` and consists of uppercase Latin letters, digits, and `_` ||
|| parameter name | Unknown parameter | Remove the parameter that is not in the description ||
|#

#### Internal Error

HTTP status: **500**

If the write fails for an internal reason, the method returns an error without details and writes none of the values. Retry the request later.

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-legal-field-list.md)
- [{#T}](./sign-b2e-template-send.md)
- [{#T}](./index.md)
