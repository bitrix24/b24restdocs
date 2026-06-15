# Register an External Call in Bitrix24 telephony.externalCall.register

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalCall.register` registers an external call in Bitrix24.

To create a CRM activity for the call, you must also call the method [telephony.externalCall.finish](./telephony-external-call-finish.md).

{% note info "" %}

The method works only in the context of an [application](../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID*** 
[`integer`](../data-types.md) | The identifier of the user for whom the call is registered.

The identifier can be obtained using the [user.get](../user/user-get.md) method. ||
|| **USER_PHONE_INNER*** 
[`string`](../data-types.md) | The internal number of the user.

The internal number can be obtained using the [user.get](../user/user-get.md) method.

{% note info "" %}

At least one of the parameters must be specified: `USER_ID` or `USER_PHONE_INNER`.

{% endnote %} ||
|| **PHONE_NUMBER***
[`string`](../data-types.md) | The client's phone number. ||
|| **TYPE***
[`integer`](../data-types.md) | The type of call.

Possible values:
- `1` — outgoing
- `2` — incoming
- `3` — incoming with redirection
- `4` — callback
- `5` — informational call. ||
|| **CALL_START_DATE**
[`string`](../data-types.md) | The date and time the call started in ISO-8601 format with timezone indication, for example, `2026-03-07T10:20:30+03:00`.

By default — the current time on the server. ||
|| **CRM_CREATE**
[`integer`](../data-types.md) | Automatic creation of a CRM object if no suitable object is found by the number.

Possible values:
- `0` — do not create
- `1` — create
  
By default — `0`. 

For outgoing calls through an external line, the final behavior also depends on the value of the `CRM_AUTO_CREATE` parameter set for the line in the methods [telephony.externalLine.add](./telephony-external-line-add.md) and [telephony.externalLine.update](./telephony-external-line-update.md). ||
|| **CRM_SOURCE**
[`string`](../data-types.md) | The identifier of the CRM source (the value of the `STATUS_ID` field).

The list of values can be obtained using the [crm.status.list](../crm/status/crm-status-list.md) method with the filter `ENTITY_ID: 'SOURCE'`. ||
|| **CRM_ENTITY_TYPE**
[`string`](../data-types.md) | The type of CRM object to associate with the call.

Possible values:
- `CONTACT` — contact
- `COMPANY` — company
- `LEAD` — lead. ||
|| **CRM_ENTITY_ID**
[`integer`](../data-types.md) | The identifier of the CRM object from `CRM_ENTITY_TYPE`.

The identifier can be obtained using the following methods:
- [crm.contact.list](../crm/contacts/crm-contact-list.md)
- [crm.company.list](../crm/companies/crm-company-list.md)
- [crm.lead.list](../crm/leads/crm-lead-list.md) ||
|| **SHOW**
[`integer`](../data-types.md) | Show the call detail form after registration.

Possible values:
- `0` — do not show
- `1` — show

By default — `1`. ||
|| **ADD_TO_CHAT**
[`integer`](../data-types.md) | Add a message about the call to the employee's chat.

Possible values:
- `0` — do not add
- `1` — add

By default — `1`. ||
|| **CALL_LIST_ID**
[`integer`](../data-types.md) | The identifier of the [call list](../crm/call-list/index.md) to which the call is linked.

If the call is initiated from a call list, pass the identifier obtained in the [ONEXTERNALCALLSTART](./events/on-external-call-start.md) event.

The list of available call lists can be obtained using the [crm.calllist.list](../crm/call-list/crm-calllist-list.md) method. ||
|| **LINE_NUMBER**
[`string`](../data-types.md) | The number of the external line.

The line number can be obtained using the [telephony.externalLine.get](./telephony-external-line-get.md) method.

This parameter is not mandatory, but it is recommended to always pass it, especially for incoming calls, to ensure proper line binding and reporting/analytics of telephony. ||
|| **EXTERNAL_CALL_ID**
[`string`](../data-types.md) | The external identifier of the call on the side of the PBX/integration.

It is recommended to pass a unique value for each physical call to avoid returning an existing `CALL_ID` when re-registering within 30 minutes. ||
|#

## Features of Re-registration

If the method `telephony.externalCall.register` is called again within 30 minutes, Bitrix24 may return an existing `CALL_ID` instead of creating a new registration.

To find an existing registration, the following technical call parameters are used:
- `PHONE_NUMBER`
- `TYPE`
- `USER_ID` or `USER_PHONE_INNER`
- `LINE_NUMBER` (if provided)
- `EXTERNAL_CALL_ID` (if provided)

The search is performed within the application and only among registrations made in the last 30 minutes.

The CRM fields and start time are not used as deduplication keys:
- `CRM_ENTITY_TYPE`
- `CRM_ENTITY_ID`
- `CALL_START_DATE`

To ensure that each physical call is registered as separate, pass a unique `EXTERNAL_CALL_ID` for each call on the side of the PBX/integration.

{% note warning "" %}

In click-to-call scenarios, where the call is created by Bitrix24 itself, the internal registration call is performed without `EXTERNAL_CALL_ID`. Therefore, in the statistics for such calls, the `EXTERNAL_CALL_ID` field is usually empty.

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":1269,"PHONE_NUMBER":"79062195047","TYPE":2,"CRM_ENTITY_TYPE":"CONTACT","CRM_ENTITY_ID":797,"SHOW":1,"LINE_NUMBER":"3","EXTERNAL_CALL_ID":"asterisk-1710140185.18441","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalCall.register
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ExternalCallRegisterResult = {
      CALL_ID: string
      CRM_CREATED_LEAD: number | null
      CRM_CREATED_ENTITIES: { ENTITY_TYPE: string; ENTITY_ID: number }[]
      CRM_ENTITY_TYPE: string
      CRM_ENTITY_ID: number
      LEAD_CREATION_ERROR?: string
    }

    try {
      const response = await $b24.actions.v2.call.make<ExternalCallRegisterResult>({
        method: 'telephony.externalCall.register',
        params: {
          USER_ID: 1269,
          PHONE_NUMBER: '79062195047',
          TYPE: 2,
          CRM_ENTITY_TYPE: 'CONTACT',
          CRM_ENTITY_ID: 797,
          SHOW: 1,
          LINE_NUMBER: '3',
          EXTERNAL_CALL_ID: 'asterisk-1710140185.18441',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.CALL_ID, result.CRM_ENTITY_TYPE, result.CRM_ENTITY_ID)
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
      async function registerExternalCall() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'telephony.externalCall.register',
            params: {
              USER_ID: 1269,
              PHONE_NUMBER: '79062195047',
              TYPE: 2,
              CRM_ENTITY_TYPE: 'CONTACT',
              CRM_ENTITY_ID: 797,
              SHOW: 1,
              LINE_NUMBER: '3',
              EXTERNAL_CALL_ID: 'asterisk-1710140185.18441',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.CALL_ID, result.CRM_ENTITY_TYPE, result.CRM_ENTITY_ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', registerExternalCall)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'telephony.externalCall.register',
                [
                    'USER_ID' => 1269,
                    'PHONE_NUMBER' => '79062195047',
                    'TYPE' => 2,
                    'CRM_ENTITY_TYPE' => 'CONTACT',
                    'CRM_ENTITY_ID' => 797,
                    'SHOW' => 1,
                    'LINE_NUMBER' => '3',
                    'EXTERNAL_CALL_ID' => 'asterisk-1710140185.18441'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error registering call: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalCall.register",
        {
            USER_ID: 1269,
            PHONE_NUMBER: '79062195047',
            TYPE: 2,
            CRM_ENTITY_TYPE: 'CONTACT',
            CRM_ENTITY_ID: 797,
            SHOW: 1,
            LINE_NUMBER: '3',
            EXTERNAL_CALL_ID: 'asterisk-1710140185.18441'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'telephony.externalCall.register',
        [
            'USER_ID' => 1269,
            'PHONE_NUMBER' => '79062195047',
            'TYPE' => 2,
            'CRM_ENTITY_TYPE' => 'CONTACT',
            'CRM_ENTITY_ID' => 797,
            'SHOW' => 1,
            'LINE_NUMBER' => '3',
            'EXTERNAL_CALL_ID' => 'asterisk-1710140185.18441'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "CALL_ID": "externalCall.716f1cb73def9700a23842adf9c4c568.1773130779",
        "CRM_CREATED_LEAD": null,
        "CRM_CREATED_ENTITIES": [],
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": 797
    },
    "time": {
        "start": 1773130778,
        "finish": 1773130779.120838,
        "duration": 1.120837926864624,
        "processing": 1,
        "date_start": "2026-03-10T11:19:38+03:00",
        "date_finish": "2026-03-10T11:19:39+03:00",
        "operating_reset_at": 1773131378,
        "operating": 0.22185301780700684
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. ||
|| **CALL_ID**
[`string`](../data-types.md) | The identifier of the call. ||
|| **CRM_CREATED_LEAD**
[`integer`](../data-types.md) | The identifier of the automatically created lead. ||
|| **CRM_CREATED_ENTITIES**
[`array`](../data-types.md) | An array of automatically created [CRM entities](#result-crm-created-entities). ||
|| **CRM_ENTITY_TYPE**
[`string`](../data-types.md) | The type of the main CRM object of the call. ||
|| **CRM_ENTITY_ID**
[`integer`](../data-types.md) | The identifier of the main CRM object of the call. ||
|| **LEAD_CREATION_ERROR**
[`string`](../data-types.md) | The error message during lead auto-creation (if any). ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

#### CRM_CREATED_ENTITIES Object {#result-crm-created-entities}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../data-types.md) | The type of the created CRM object. ||
|| **ENTITY_ID**
[`integer`](../data-types.md) | The identifier of the created CRM object. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Unknown TYPE"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method. | Method called outside the application context. ||
|| `ERROR_CORE` | USER_ID or USER_PHONE_INNER should be set. | `USER_ID` and `USER_PHONE_INNER` not provided. ||
|| `ERROR_CORE` | Unknown TYPE. | Invalid value for `TYPE` provided. ||
|| `ERROR_CORE` | CALL_START_DATE should be in the ISO-8601 format. | Incorrect format for `CALL_START_DATE`. ||
|| `ERROR_CORE` | Unsupported phone number format. | Incorrect format for `PHONE_NUMBER`. ||
|| `ERROR_CORE` | User is not found or is not active. | User not found or inactive. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-call-search-crm-entities.md)
- [{#T}](./telephony-external-call-show.md)
- [{#T}](./telephony-external-call-hide.md)
- [{#T}](./telephony-external-call-finish.md)
- [{#T}](./telephony-external-call-attach-record.md)