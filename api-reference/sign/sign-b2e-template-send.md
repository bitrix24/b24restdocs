# Send a Document Using a Template sign.b2e.template.send

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`, `crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to create, view, and edit e-Signature for HR documents and permission to view the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The `sign.b2e.template.send` method creates a document from a company template and immediately sends it to an employee for signing. One call creates one document for one signer.

The method works in an application and via an inbound webhook. The application or webhook needs two scopes: `sign.b2e` and `crm`. The method cannot be called inside a [batch](../rest-v3.md#batch).

{% note warning "" %}

A repeated call may send the employee a second document. Pass the `Idempotency-Key` header with an idempotency key, as in the cURL tabs: a repeated call with the same key and the same request body returns the response of the first call and does not send a second document. Use a new key for each new document. For details, see [Repeated Calls Without Duplicates](../rest-v3.md#idempotency).

Bitrix24 stores only a successful response under the key. After an error, a repeated call with the same key runs again as a new call.

The response may take tens of seconds to arrive: wait for it before repeating the call. You can repeat the call right away only after the `429` status: such a call does not create a document. After the `500` or `503` status, a proxy error, or a client timeout, first check in Bitrix24 whether the document has appeared among the employee's documents: such a response does not tell you whether the document was created.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateUid***
[`string`](../data-types.md) | Template identifier. You can retrieve it using the [sign.b2e.template.list](./sign-b2e-template-list.md) method or from the contents of a bundle using the [sign.b2e.template.folder.get](./sign-b2e-template-folder-get.md) method ||
|| **signerUserId***
[`integer`](../data-types.md) | Identifier of the user who signs the document. The signer can be an active employee or an active extranet user, for example, a candidate who has not been hired yet.

You can retrieve the identifier using the [user.get](../user/user-get.md) method ||
|| **responsibleUserId**
[`integer`](../data-types.md) | Identifier of the person responsible for the document. The responsible person can be an active employee with permission to create e-Signature for HR documents and permission to view this template.

Default: the user on whose behalf the method is called ||
|| **externalSettings**
[`object`](../data-types.md) | Document number and date in an external system [(detailed description)](#external-settings) ||
|#

The method does not accept other parameters: a call with them returns a validation error. The exception is the application authorization parameters `auth`, `access_token`, `auth_connector`, and `state`: you can pass them in the request body.

### Parameter externalSettings {#external-settings}

#|
|| **Name**
`type` | **Description** ||
|| **externalId**
[`string`](../data-types.md) | External document number. Length: from 1 to 255 characters ||
|| **externalDateCreate**
[`string`](../data-types.md) | Document date in the external system in the `YYYY-MM-DDThh:mm:ss+hh:mm` format, for example, `2026-11-12T10:00:00+01:00`. The date and time are stored as passed, without time zone conversion ||
|#

If the document number in the template is filled in by the ERP integration, do not pass `externalId`. If the date is filled in by the integration, do not pass `externalDateCreate`. Otherwise, the method returns the `SIGN_B2E_EXTERNAL_SETTINGS_FROM_INTEGRATION` error.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/sign.b2e.template.send`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Idempotency-Key: 6f1d2c3b-8a4e-4b7f-9c1d-2e3f4a5b6c7d" \
    -d '{"templateUid":"e2bjlmefhz8tzy1klbdnpnfsk3np08l2","signerUserId":1595,"externalSettings":{"externalId":"HR-2026-118","externalDateCreate":"2026-11-12T10:00:00+01:00"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/sign.b2e.template.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Idempotency-Key: 6f1d2c3b-8a4e-4b7f-9c1d-2e3f4a5b6c7d" \
    -d '{"templateUid":"e2bjlmefhz8tzy1klbdnpnfsk3np08l2","signerUserId":1595,"externalSettings":{"externalId":"HR-2026-118","externalDateCreate":"2026-11-12T10:00:00+01:00"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/sign.b2e.template.send
    ```

- JS (TS)

    REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type State = {
      code: string
      name: string
    }

    type TemplateSendResult = {
      item: {
        uid: string
        state: State
        members: Array<{
          uid: string
          role: string
          party: number
          user: {
            employeeCode: string | null
            employeeId: number | null
            userId: number | null
          }
          state: State
        }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateSendResult>(
        'sign.b2e.template.send',
        {
          templateUid: 'e2bjlmefhz8tzy1klbdnpnfsk3np08l2',
          signerUserId: 1595,
          externalSettings: {
            externalId: 'HR-2026-118',
            externalDateCreate: '2026-11-12T10:00:00+01:00',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document uid:', result.item.uid, 'state:', result.item.state.code)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 `sign.b2e.*` methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function sendByTemplate() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'sign.b2e.template.send',
            {
              templateUid: 'e2bjlmefhz8tzy1klbdnpnfsk3np08l2',
              signerUserId: 1595,
              externalSettings: {
                externalId: 'HR-2026-118',
                externalDateCreate: '2026-11-12T10:00:00+01:00',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document uid:', result.item.uid, 'state:', result.item.state.code)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', sendByTemplate)
    </script>
    ```

- Python

    The SDK sends only the methods from the `Config().api_v3_methods` list to REST 3.0, and the `sign.b2e.*` methods are not in it yet. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "sign.b2e.template.send"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateUid": "e2bjlmefhz8tzy1klbdnpnfsk3np08l2",
                "signerUserId": 1595,
                "externalSettings": {
                    "externalId": "HR-2026-118",
                    "externalDateCreate": "2026-11-12T10:00:00+01:00",
                },
            },
            prefer_version=3,
        )
        document = response["result"]["item"]
        print(document["uid"], document["state"]["code"])
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

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sign.b2e.template.send',
                [
                    'templateUid' => 'e2bjlmefhz8tzy1klbdnpnfsk3np08l2',
                    'signerUserId' => 1595,
                    'externalSettings' => [
                        'externalId' => 'HR-2026-118',
                        'externalDateCreate' => '2026-11-12T10:00:00+01:00',
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

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'sign.b2e.template.send',
        {
            templateUid: 'e2bjlmefhz8tzy1klbdnpnfsk3np08l2',
            signerUserId: 1595,
            externalSettings: {
                externalId: 'HR-2026-118',
                externalDateCreate: '2026-11-12T10:00:00+01:00',
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

    The SDK does not yet support calls to the `/rest/api/` address. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.template.send',
        [
            'templateUid' => 'e2bjlmefhz8tzy1klbdnpnfsk3np08l2',
            'signerUserId' => 1595,
            'externalSettings' => [
                'externalId' => 'HR-2026-118',
                'externalDateCreate' => '2026-11-12T10:00:00+01:00',
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
    res, err := client.Core().Call(ctx, "sign.b2e.template.send", b24.Params{
    	"templateUid":  "e2bjlmefhz8tzy1klbdnpnfsk3np08l2",
    	"signerUserId": 1595,
    	"externalSettings": b24.Params{
    		"externalId":         "HR-2026-118",
    		"externalDateCreate": "2026-11-12T10:00:00+01:00",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sign.b2e.template.send: %w", err)
    }

    var result struct {
    	Item struct {
    		UID   string `json:"uid"`
    		State struct {
    			Code string `json:"code"`
    			Name string `json:"name"`
    		} `json:"state"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.UID, result.Item.State.Code)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "uid": "R-1U-GZR3-CUI1-AWAV",
            "state": {
                "code": "new",
                "name": "New"
            },
            "members": [
                {
                    "uid": "sasx13zpk23ldaj0js8lbo689mzq7djg",
                    "role": "assignee",
                    "party": 1,
                    "user": {
                        "employeeCode": null,
                        "employeeId": null,
                        "userId": 1295
                    },
                    "state": {
                        "code": "wait",
                        "name": "Pending"
                    }
                },
                {
                    "uid": "rqylyeik0eyn2am884ug2hgctqz9c5qs",
                    "role": "signer",
                    "party": 2,
                    "user": {
                        "employeeCode": null,
                        "employeeId": null,
                        "userId": 1595
                    },
                    "state": {
                        "code": "wait",
                        "name": "Pending"
                    }
                }
            ]
        }
    },
    "time": {
        "start": 1794480677,
        "finish": 1794480678.819885,
        "duration": 1.819885015487671,
        "processing": 1,
        "date_start": "2026-11-12T11:51:17+01:00",
        "date_finish": "2026-11-12T11:51:18+01:00",
        "operating_reset_at": 1794481260,
        "operating": 1.2076218128204346
    }
}
```

The response has the same structure as the response of the [sign.b2e.document.get](./sign-b2e-document-get.md) method: you can parse the sending result and subsequent status checks with the same code.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with response data ||
|| **result.item**
[`object`](../data-types.md) | The sent document [(detailed description)](#item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **uid**
[`string`](../data-types.md) | Document identifier. Use it to retrieve the document status with the [sign.b2e.document.get](./sign-b2e-document-get.md) method ||
|| **state**
[`object`](../data-types.md) | Document status [(detailed description)](#state). Right after sending, the status is `new`; then the document moves to `signing` ||
|| **members**
[`array`](../data-types.md) | Signing participants [(detailed description)](#members).

An empty array is returned if the document was sent but could not be read after sending. In this case, retrieve the participants with the [sign.b2e.document.get](./sign-b2e-document-get.md) method by `uid` ||
|#

#### State Object {#state}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Status code. Check the status by its code, not by its name.

Document status codes:

- `new` — the document has been created, signing has not started yet
- `signing` — signing is in progress
- `stopped` — signing has been stopped
- `done` — the document has been signed

Participant status codes:

- `wait` — waiting for their turn
- `ready` — signing the document
- `done` — has signed
- `refused` — refused to sign
- `stopped` — signing has been stopped ||
|| **name**
[`string`](../data-types.md) | Status name in the response language ||
|#

#### Members Array Element {#members}

#|
|| **Name**
`type` | **Description** ||
|| **uid**
[`string`](../data-types.md) | Participant identifier ||
|| **role**
[`string`](../data-types.md) | Participant role.

Possible values:

- `signer` — the employee who signs the document
- `assignee` — company representative
- `reviewer` — approver
- `editor` — editor ||
|| **party**
[`integer`](../data-types.md) | Number of the signing party the participant belongs to ||
|| **user**
[`object`](../data-types.md) | User data [(detailed description)](#user) ||
|| **state**
[`object`](../data-types.md) | Participant status. The fields are the same as for the [document status](#state) ||
|#

#### User Object {#user}

#|
|| **Name**
`type` | **Description** ||
|| **employeeCode**
[`string`](../data-types.md) or `null` | Employee code in the HR system. `null` if the employee is not linked to HCM Link, the e-Signature for HR integration with an HR system ||
|| **employeeId**
[`integer`](../data-types.md) or `null` | Employee identifier in the HR system. `null` if the employee is not linked to HCM Link ||
|| **userId**
[`integer`](../data-types.md) or `null` | Bitrix24 user identifier ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "SIGN_B2E_TEMPLATE_NOT_FOUND",
        "message": "Template not found or unavailable for sending. Select one of the available templates.",
        "validation": [
            {
                "field": "templateUid",
                "code": "SIGN_B2E_TEMPLATE_NOT_FOUND"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../_includes/error-info-v3.md) %}

If the error relates to a parameter, the `error.validation` array names it in the `field` field and repeats the error code in the `code` field.

### Possible Error Codes

#### Access Errors

HTTP status: **403**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_ACCESS_DENIED` | Insufficient permissions to perform this action in HR documents. Please contact your Bitrix24 administrator | Grant the user permissions to create, view, and edit e-Signature for HR documents. The method must be called by an active employee: a service account, an extranet user, and a collab guest receive this error ||
|| `SIGN_REST_SCOPE_REQUIRED` | Missing access permission: crm. Add it in the settings of your app or inbound webhook | Add the `crm` scope to the application or webhook ||
|| `SIGN_REST_AUTH_TYPE_NOT_ALLOWED` | This method is available to apps and inbound webhooks only | Call the method from an application or via an inbound webhook ||
|| `SIGN_B2E_UNAVAILABLE` | HR documents are not available in this Bitrix24 | Check that e-Signature for HR is available in your Bitrix24 ||
|| `SIGN_B2E_TEMPLATES_UNAVAILABLE` | Templates for HR documents are not available in this Bitrix24 | Check that e-Signature for HR templates are available in your Bitrix24 ||
|| `SIGN_B2E_TARIFF_RESTRICTED` | HR documents are not available on your current plan | Change your Bitrix24 plan ||
|#

#### Template Errors

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_TEMPLATE_NOT_FOUND` | Template not found or unavailable for sending. Select one of the available templates | Take `uid` from the [sign.b2e.template.list](./sign-b2e-template-list.md) response. The error also occurs when the template exists but the user cannot view it. HTTP status **404** ||
|| `SIGN_B2E_TEMPLATE_COMPANY_NOT_FOUND` | The company associated with the template was not found | Specify an existing company in the template or select another template. HTTP status **409** ||
|| `SIGN_B2E_TEMPLATE_FILE_INVALID` | The template file is unavailable. Replace it in Bitrix24 or select another template | Upload the template file to Bitrix24 again. HTTP status **409** ||
|#

#### Participant Errors

HTTP status: **400**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_SIGNER_NOT_FOUND` | The user was not found or cannot sign documents. Specify another signer | Pass an active employee or an active extranet user in `signerUserId` ||
|| `SIGN_B2E_RESPONSIBLE_NOT_ALLOWED` | Cannot assign this employee as the responsible person. Specify an employee with permission to send documents using this template | Pass in `responsibleUserId` an active employee with permission to create e-Signature for HR documents and permission to view the template ||
|| `SIGN_B2E_MEMBER_ROLE_NOT_RESOLVED` | No employee with the role specified in the template was found for this signer. Check the company structure or specify another signer | The template defines the participant by role, for example, the signer's supervisor. Check the company structure or specify another signer ||
|#

#### External Settings Error

HTTP status: **409**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_EXTERNAL_SETTINGS_FROM_INTEGRATION` | This field is populated from 1C based on the template settings. Remove it from the request | Remove the field named in `error.validation[].field` from `externalSettings` ||
|#

#### Batch Call Error

HTTP status: **400**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_REST_BATCH_NOT_SUPPORTED` | This method cannot be called in a batch request. Call it separately | Call the method in a separate request, not inside `batch` ||
|#

#### Signing Service Error

HTTP status: **503**

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `SIGN_B2E_SIGNING_SERVICE_TEMPORARY_ERROR` | The signing service is temporarily unavailable. The document was not sent. Please retry the request later | Check the employee's documents in Bitrix24. If the document is not there, retry the request later ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `templateUid` | Required field `templateUid` is missing | Add `templateUid` to the request body ||
|| `templateUid` | A string is expected | Pass the template identifier as a string ||
|| `signerUserId` | Required field `signerUserId` is missing | Add `signerUserId` to the request body ||
|| `signerUserId`, `responsibleUserId` | An integer is expected | Pass the identifier as a number ||
|| `responsibleUserId` | Field value must be 1 or greater | Pass a user identifier greater than `0` or omit the parameter ||
|| `externalSettings.externalId` | Field value must not be empty | Pass a non-empty document number or omit the field ||
|| `externalSettings.externalDateCreate` | A date in the format YYYY-MM-DDThh:mm:ss+hh:mm is expected | Pass the date in the `2026-11-12T10:00:00+01:00` format ||
|| parameter name | Unknown parameter | Remove the parameter that is not listed in the description ||
|#

#### Idempotency Key Errors

#|
|| **Code** | **Error description** | **How to Fix** ||
|| `BITRIX_REST_V3_EXCEPTION_IDEMPOTENCYKEYREUSEDEXCEPTION` | The key was already used in a successful call with a different request body | Use a new key for a new document. Send the previous key only with the same request body. HTTP status **422** ||
|| `BITRIX_REST_V3_EXCEPTION_INVALIDIDEMPOTENCYKEYEXCEPTION` | The key value does not fit the length limit or contains invalid characters | Pass a string of 1 to 255 printable ASCII characters. HTTP status **400** ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-template-list.md)
- [{#T}](./sign-b2e-template-folder-get.md)
- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./events/on-sign-b2e-document-status-changed.md)
- [{#T}](./index.md)
