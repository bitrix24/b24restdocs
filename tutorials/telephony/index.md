# How to Integrate External Telephony with Bitrix24

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

External telephony transmits call data from the PBX to Bitrix24: client number, user, line, call status, and recording. Bitrix24 displays the call detail form to the employee, links the call to the CRM, and saves the result in the statistics.

To integrate external telephony, we will follow six steps:

1. Build the application and handlers for the PBX and Bitrix24
2. Register the incoming call
3. Display the call detail form to a group of employees
4. Route the call to the responsible manager
5. Handle the outgoing call from the CRM
6. Complete the call and save the result

We will also discuss a scenario where the call needs to be recorded without displaying the detail form.

## 1. Build the Application

A working integration typically consists of two parts:

- server application
- handlers for the PBX and Bitrix24

1. Create a [local application](../../settings/app-installation/local-apps/index.md) or an application for the Marketplace.
2. Complete the application installation and save the authorization according to the [application installation rules](../../settings/app-installation/installation-finish.md).
3. Register the external line using the [telephony.externalLine.add](../../api-reference/telephony/telephony-external-line-add.md) method. Pass the line number in the `LINE_NUMBER` parameter of the [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) method.
4. Subscribe the application to [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) using the [event.bind](../../api-reference/events/event-bind.md) method if you need to initiate outgoing calls from the CRM.
5. Create a handler for incoming events from the PBX. It should call methods based on the call status:
   - [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) — register the call
   - [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) — display the call detail form
   - [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md) — hide the call detail form
   - [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) — complete the call
6. Create a handler for [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md). It should accept the following event data:
   - `CALL_ID` — the identifier of the call that needs to be completed after the conversation
   - `PHONE_NUMBER` — the client number to call
   - `USER_ID` — the identifier of the employee who initiated the call

   After that, the handler initiates the call on the PBX side and completes the same `CALL_ID` using the [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) method.
7. If the call recording is available after the call is completed, attach it using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method.

For PHP applications, you can use the [CRest PHP SDK](../../sdk/crest-php-sdk/index.md). Do not store tokens in a public application file and do not disable SSL certificate verification for REST requests.

## 2. Register the Incoming Call

When the PBX receives an incoming call, invoke the [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) method. Pass the following parameters:

- `USER_ID` — the identifier of the employee to whom the call detail form should be displayed
- `PHONE_NUMBER` — the client number
- `TYPE = 2` — incoming call
- `LINE_NUMBER` — the external line number
- `EXTERNAL_CALL_ID` — a unique identifier for the call on the PBX side

The method will return a `CALL_ID`. This identifier is needed for subsequent actions with the call:

- display the detail form using [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md)
- hide the detail form using [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md)
- complete the call using [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md)
- add a recording using [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md)

If you pass `SHOW = 1` or do not pass `SHOW`, the detail form will open for the user specified in `USER_ID`.

## 3. Display the Call to a Group of Employees

For a queue of operators, first register the call using the [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) method, and then manage the detail form using `CALL_ID`.

For the queue, you can register the call for the first operator and then display the detail form to other employees using the [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) method.

**Simultaneous Queue.** Pass an array of employee identifiers in the `USER_ID` parameter of the [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) method. Bitrix24 will display the detail form to multiple employees. When the PBX selects an operator, hide the detail form from the others using the [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md) method.

**Sequential Queue.** Display the detail form to the first employee using the [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) method. If the employee does not answer within the time set in the PBX, hide the detail form using the [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md) method and show it to the next employee using the [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) method.

In the example, the detail form is first shown to three employees. When the employee with identifier `1270` answers the call, the detail form is hidden from the others.

{% list tabs %}

- JS

    ```js
    const queue = [1269, 1270, 1271];

    await $b24.callMethod(
        'telephony.externalCall.show',
        {
            CALL_ID: callId,
            USER_ID: queue
        }
    );

    const answeredUserId = 1270;
    const usersToHide = queue.filter((userId) => userId !== answeredUserId);

    await $b24.callMethod(
        'telephony.externalCall.hide',
        {
            CALL_ID: callId,
            USER_ID: usersToHide
        }
    );
    ```

- PHP

    ```php
    $queue = [1269, 1270, 1271];

    CRest::call(
        'telephony.externalCall.show',
        [
            'CALL_ID' => $callId,
            'USER_ID' => $queue
        ]
    );

    $answeredUserId = 1270;
    $usersToHide = array_values(
        array_filter(
            $queue,
            fn($userId) => $userId !== $answeredUserId
        )
    );

    CRest::call(
        'telephony.externalCall.hide',
        [
            'CALL_ID' => $callId,
            'USER_ID' => $usersToHide
        ]
    );
    ```

{% endlist %}

## 4. Route the Call to the Responsible Manager

If you need to display the incoming call to the responsible manager, first register the call with `SHOW = 0` using the [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) method. Bitrix24 will check the number in the CRM and return the data of the found or created object:

- `CRM_ENTITY_TYPE` — the type of the found CRM object
- `CRM_ENTITY_ID` — the identifier of the found CRM object
- `CRM_CREATED_LEAD` — the identifier of the created lead if auto-creation is enabled
- `CRM_CREATED_ENTITIES` — an array of created CRM objects if auto-creation is enabled

Using the found CRM object, retrieve the responsible employee using the CRM methods and pass their identifier to the [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) method. If you only need to find the client by phone without registering the call, use [telephony.externalCall.searchCrmEntities](../../api-reference/telephony/telephony-external-call-search-crm-entities.md).

## 5. Handle the Outgoing Call from the CRM

When an employee clicks on a phone number in the CRM, Bitrix24 registers the call and sends the application the [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) event. The event handler receives:

- `PHONE_NUMBER` — the client number
- `USER_ID` — the identifier of the employee who initiated the call
- `CALL_ID` — the identifier of the registered call
- `LINE_NUMBER` — the external line number
- `CRM_ENTITY_TYPE` — the type of the CRM object from which the call was initiated
- `CRM_ENTITY_ID` — the identifier of the CRM object
- `CALL_LIST_ID` — the identifier of the call list if the call was initiated from a call list

After receiving the [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) event, the application should initiate the call on the PBX side. When the conversation ends, complete the same `CALL_ID` using the [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) method.

If the call is from a call list, use the `CALL_LIST_ID` from the [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) when processing the call on the application side. Complete the call using the `CALL_ID` from the event so that the result is saved in connection with the call list.

## 6. Complete the Call and Save the Result

After the conversation ends, invoke the [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) method. This method hides the call detail form, saves the record in the statistics, and creates or updates the CRM activity for the call.

Pass the following parameters to [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md):

- `CALL_ID` — the identifier from [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) or [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md)
- `USER_ID` — the employee for whom the call should be saved
- `DURATION` — the duration of the conversation in seconds
- `STATUS_CODE` — the result of the call, for example, `200` for a successful conversation or `304` for a missed incoming call

If the recording of the conversation is not yet ready, use one of two approaches:

- invoke [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) without the recording to immediately save the call in the statistics and CRM activity. When the recording is ready, attach it using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method.
- hide the detail form using the [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md) method, and invoke [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) after the recording is ready. In this case, the call will appear in the statistics and CRM activity only after invoking [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md).

After attaching the recording, you can add a transcription using the [telephony.call.attachTranscription](../../api-reference/telephony/telephony-call-attach-transcription.md) method. This method works only for completed calls that are already in the statistics.

## Record the Call Without Displaying the Detail Form

If the connection between the PBX and Bitrix24 was unavailable during the call, you can save the call record without the detail form after the connection is restored. To do this, invoke [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) with `SHOW = 0`, and then [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md) with the actual call data.

This scenario does not show the call to the employee in real-time but preserves the history, statistics, and CRM activity.

## Example

The example demonstrates the minimal cycle for an incoming call: registration, displaying the detail form to another employee, and completion.

{% list tabs %}

- JS

    ```js
    const registerResult = await $b24.callMethod(
        'telephony.externalCall.register',
        {
            USER_ID: 1269,
            PHONE_NUMBER: '19061234567',
            TYPE: 2,
            LINE_NUMBER: '3',
            EXTERNAL_CALL_ID: 'asterisk-1773130778.18441',
            SHOW: 1
        }
    );

    const callId = registerResult.getData().result.CALL_ID;

    await $b24.callMethod(
        'telephony.externalCall.show',
        {
            CALL_ID: callId,
            USER_ID: 1270
        }
    );

    await $b24.callMethod(
        'telephony.externalCall.finish',
        {
            CALL_ID: callId,
            USER_ID: 1270,
            DURATION: 95,
            STATUS_CODE: '200',
            ADD_TO_CHAT: 1
        }
    );
    ```

- PHP

    ```php
    $registerResult = CRest::call(
        'telephony.externalCall.register',
        [
            'USER_ID' => 1269,
            'PHONE_NUMBER' => '19061234567',
            'TYPE' => 2,
            'LINE_NUMBER' => '3',
            'EXTERNAL_CALL_ID' => 'asterisk-1773130778.18441',
            'SHOW' => 1
        ]
    );

    $callId = $registerResult['result']['CALL_ID'];

    CRest::call(
        'telephony.externalCall.show',
        [
            'CALL_ID' => $callId,
            'USER_ID' => 1270
        ]
    );

    CRest::call(
        'telephony.externalCall.finish',
        [
            'CALL_ID' => $callId,
            'USER_ID' => 1270,
            'DURATION' => 95,
            'STATUS_CODE' => '200',
            'ADD_TO_CHAT' => 1
        ]
    );
    ```

{% endlist %}

## Continue Your Learning

- [Overview of Telephony Methods](../../api-reference/telephony/index.md)
- [Telephony Events](../../api-reference/telephony/events/index.md)
- [CALL_CARD Tab in the Call Detail Form](../../api-reference/widgets/telephony/index.md)