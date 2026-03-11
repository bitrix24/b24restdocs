# Finish Call and Record It in Telephony Statistics telephony.externalCall.finish

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalCall.finish` ends an external call, saves it in the statistics, and in the CRM activity.

{% note info "" %}

The method works only in the context of the [application](../../settings/app-installation/index.md)

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID***
[`string`](../data-types.md) | Identifier of the call from the method [telephony.externalCall.register](./telephony-external-call-register.md). ||
|| **USER_ID***
[`integer`](../data-types.md) | Identifier of the user who is finishing the call.

The identifier can be obtained using the method [user.get](../user/user-get.md) ||
|| **USER_PHONE_INNER***
[`string`](../data-types.md) | Internal number of the user.

The internal number can be obtained using the method [user.get](../user/user-get.md)

{% note info "" %}

At least one of the parameters must be specified: `USER_ID` or `USER_PHONE_INNER`

{% endnote %} ||
|| **DURATION**
[`integer`](../data-types.md) | Duration of the call in seconds.

Default is `0` ||
|| **COST**
[`double`](../data-types.md) | Cost of the call.

Default is `0` ||
|| **COST_CURRENCY**
[`string`](../data-types.md) | Currency of the call cost.

The list of currencies can be obtained using the method [crm.currency.list](../crm/currency/crm-currency-list.md).

Default is an empty string ||
|| **STATUS_CODE**
[`string`](../data-types.md) | Result code of the call.

Possible values:
- `200` — successful call
- `304` — missed call
- `603` — declined
- `603-S` — call canceled
- `403` — forbidden
- `404` — invalid number
- `486` — busy
- `484` — direction unavailable
- `503` — direction unavailable
- `480` — temporarily unavailable
- `402` — insufficient funds
- `423` — blocked
- `OTHER` — undefined

Default:
- `200`, if `DURATION > 0`
- `304`, if `DURATION = 0` ||
|| **FAILED_REASON**
[`string`](../data-types.md) | Text reason for the failed call.

Default is an empty string ||
|| **RECORD_URL**
[`string`](../data-types.md) | URL of the call recording.

This parameter is deprecated and retained for backward compatibility. It is recommended to use [telephony.externalCall.attachRecord](./telephony-external-call-attach-record.md) ||
|| **VOTE**
[`integer`](../data-types.md) | Rating of the call.

Possible values:
- `1`, `2`, `3`, `4`, `5`

If the rating is absent — `0` or `null` ||
|| **ADD_TO_CHAT**
[`integer`](../data-types.md) | Add a message about the call to the employee's chat.

Possible values:
- `0` — do not add
- `1` — add

Default is `1` ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","USER_ID":1269,"DURATION":95,"STATUS_CODE":"200","VOTE":5,"ADD_TO_CHAT":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalCall.finish
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalCall.finish',
            {
                CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                USER_ID: 1269,
                DURATION: 95,
                STATUS_CODE: '200',
                VOTE: 5,
                ADD_TO_CHAT: 0
            }
        );
        
        const result = response.getData().result;
        console.log('Call finished:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'telephony.externalCall.finish',
                [
                    'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                    'USER_ID' => 1269,
                    'DURATION' => 95,
                    'STATUS_CODE' => '200',
                    'VOTE' => 5,
                    'ADD_TO_CHAT' => 0
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error finishing call: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalCall.finish",
        {
            CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            USER_ID: 1269,
            DURATION: 95,
            STATUS_CODE: '200',
            VOTE: 5,
            ADD_TO_CHAT: 0
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
        'telephony.externalCall.finish',
        [
            'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            'USER_ID' => 1269,
            'DURATION' => 95,
            'STATUS_CODE' => '200',
            'VOTE' => 5,
            'ADD_TO_CHAT' => 0
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
        "EXTERNAL_CALL_ID": null,
        "PORTAL_USER_ID": 1269,
        "PHONE_NUMBER": "19062195047",
        "PORTAL_NUMBER": "3",
        "INCOMING": "2",
        "CALL_DURATION": 95,
        "CALL_START_DATE": {},
        "CALL_STATUS": 1,
        "CALL_VOTE": 5,
        "COST": 0,
        "COST_CURRENCY": "",
        "CALL_FAILED_CODE": "200",
        "CALL_FAILED_REASON": "",
        "REST_APP_ID": "3",
        "REST_APP_NAME": "REST API Documentation",
        "CRM_ACTIVITY_ID": 7943,
        "COMMENT": null,
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": 797,
        "ID": 7
    },
    "time": {
        "start": 1773132478,
        "finish": 1773132480.301376,
        "duration": 2.3013761043548584,
        "processing": 2,
        "date_start": "2026-03-10T11:47:58+01:00",
        "date_finish": "2026-03-10T11:48:00+01:00",
        "operating_reset_at": 1773133078,
        "operating": 1.4118058681488037
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Data of the saved call record ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the record in the statistics ||
|| **CALL_ID**
[`string`](../data-types.md) | Identifier of the call ||
|| **EXTERNAL_CALL_ID**
[`string`](../data-types.md) | External identifier of the call on the integration side ||
|| **PORTAL_USER_ID**
[`integer`](../data-types.md) | Identifier of the Bitrix24 user on behalf of whom the call was finished ||
|| **PHONE_NUMBER**
[`string`](../data-types.md) | Subscriber's number ||
|| **PORTAL_NUMBER**
[`string`](../data-types.md) | Line number through which the call was made ||
|| **INCOMING**
[`string`](../data-types.md) | Type of call.

Possible values:
- `1` — outgoing
- `2` — incoming
- `3` — incoming with redirection
- `4` — callback
- `5` — informational call ||
|| **CALL_DURATION**
[`integer`](../data-types.md) | Duration of the call in seconds ||
|| **CALL_START_DATE**
[`datetime`](../data-types.md) | Date and time when the call started ||
|| **CALL_STATUS**
[`integer`](../data-types.md) | Completion status.

Possible values:
- `1` — successful conversation
- `0` — unsuccessful/missed ||
|| **CALL_VOTE**
[`integer`](../data-types.md) | Rating of the call ||
|| **COST**
[`double`](../data-types.md) | Cost of the call ||
|| **COST_CURRENCY**
[`string`](../data-types.md) | Currency of the cost ||
|| **CALL_FAILED_CODE**
[`string`](../data-types.md) | Call completion code.

Possible values:
- `200` — successful call
- `304` — missed call
- `603` — declined
- `603-S` — call canceled
- `403` — forbidden
- `404` — invalid number
- `486` — busy
- `484` — direction unavailable
- `503` — direction unavailable
- `480` — temporarily unavailable
- `402` — insufficient funds
- `423` — blocked
- `OTHER` — undefined ||
|| **CALL_FAILED_REASON**
[`string`](../data-types.md) | Text reason for the call completion ||
|| **REST_APP_ID**
[`integer`](../data-types.md) | Application identifier ||
|| **REST_APP_NAME**
[`string`](../data-types.md) | Name of the application ||
|| **CRM_ACTIVITY_ID**
[`integer`](../data-types.md) | Identifier of the CRM activity for the call ||
|| **COMMENT**
[`string`](../data-types.md) | Comment on the call ||
|| **CRM_ENTITY_TYPE**
[`string`](../data-types.md) | Type of CRM entity associated with the call ||
|| **CRM_ENTITY_ID**
[`integer`](../data-types.md) | Identifier of the CRM entity associated with the call ||
|| **ERRORS**
[`object`](../data-types.md) | Additional processing errors (if any) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "INVALID_ARGUMENT",
    "error_description": "CALL_ID must be a string"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Method called outside the application context ||
|| `INVALID_ARGUMENT` | CALL_ID must be a string | Parameter `CALL_ID` must be a string ||
|| `ERROR_CORE` | USER_ID or USER_PHONE_INNER should be set | `USER_ID` and `USER_PHONE_INNER` not provided ||
|| `ERROR_CORE` | Call is not found (call should be registered prior to finishing) | Call not found, it must be registered using the method `telephony.externalCall.register` before finishing ||
|| `ERROR_CORE` | User is not found or is not active | User not found or inactive ||
|| `ERROR_CORE` | Unexpected database error | Error saving statistics ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-call-register.md)
- [{#T}](./telephony-external-call-attach-record.md)
- [{#T}](./telephony-external-call-show.md)
- [{#T}](./telephony-external-call-hide.md)