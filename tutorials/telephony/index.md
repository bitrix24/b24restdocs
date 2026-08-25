# How to Integrate External Telephony with Bitrix24

> Scope: [`telephony`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: to complete the full scenario, you need an installed application with OAuth authorization
>
> - [telephony.externalLine.add](../../api-reference/telephony/telephony-external-line-add.md), [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md), [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md), and [event.bind](../../api-reference/events/event-bind.md) — the user under whom the application received OAuth authorization
> - [telephony.externalCall.searchCrmEntities](../../api-reference/telephony/telephony-external-call-search-crm-entities.md), [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md), [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md), [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md), and [telephony.call.attachTranscription](../../api-reference/telephony/telephony-call-attach-transcription.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

External telephony transmits call data from the PBX to Bitrix24: client number, user, line, call status, and recording. Bitrix24 displays the call card to the employee, links the call to the CRM, and retains the result in statistics.

The scenario consists of six steps.

1. Assemble the application and handlers for the PBX and Bitrix24
2. Register an incoming call
3. Display the call card to an employee group
4. Route the call to the customer's responsible person
5. Process an outgoing call from the CRM
6. Finish the call and retain the result

We will separately examine a scenario where a call must be recorded without displaying a card.

{% note info "" %}

The `telephony.externalLine.add`, `telephony.externalCall.register`, and `telephony.externalCall.finish` methods work only in the application context. An inbound webhook is not suitable for them. The [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) event (step 5) is received by your web server.

In PHP, telephony methods are called directly through the core (`$b24->core->call(...)`). Typed analogs are available in `getTelephonyScope()->externalCall()` (`show`, `hide`, `register`, `finishForUserId`) and `->externalLine()`, but they require value objects (`CallType`, `TelephonyCallStatusCode`, `Money`, `CarbonImmutable`).

{% endnote %}

## Before You Start

Before you begin, make sure you have:

- a local or mass-market application with the `telephony` scope and retained OAuth authorization
- a public HTTPS application handler if you need to receive the `ONEXTERNALCALLSTART` event. An inbound webhook does not receive this event
- the employee identifier `USER_ID` to whom the call card will be shown
- the external line number `LINE_NUMBER`, for example `line-1`
- the unique call identifier on the PBX side, `EXTERNAL_CALL_ID`
- a public URL of the call recording if the recording is passed using the `telephony.externalCall.attachRecord` method

Replace `your-domain.bitrix24.com`, `1269`, `1270`, `1271`, `line-1`, `asterisk-1773130778.18441`, and the recording URL with the data of your Bitrix24, employees, lines, and PBX.

{% include [Note on examples](../../_includes/examples.md) %}

## SDK Initialization

In the examples below, `$b24` for JS, `$b24` for PHP, and `client` for Python are initialized clients with the OAuth token of the installed application. Receiving, storing, and refreshing OAuth tokens are described in [Full OAuth 2.0 Authorization Protocol](../../settings/oauth/index.md).

## 1. Assemble the Application

A working integration typically consists of a server-side application and handlers for the PBX and Bitrix24:

1. Create a [local application](../../settings/app-installation/local-apps/index.md) or a Market application
2. Complete the application installation and retain the authorization
3. Register an external line using the [telephony.externalLine.add](../../api-reference/telephony/telephony-external-line-add.md) method. Pass the line number in `LINE_NUMBER` of the [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) method
4. Subscribe the application to [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) using the [event.bind](../../api-reference/events/event-bind.md) method if you need to initiate outgoing calls from the CRM
5. Create an event handler from the PBX that calls `telephony.externalCall.register/show/hide/finish` based on the call status
6. Create an [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) handler for outgoing calls
7. If the call recording appears after completion, attach it using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method

Registering an external line creates a number that links calls to the application. Pass this number in `LINE_NUMBER` when registering a call.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'telephony.externalLine.add',
        params: { NUMBER: 'line-1', NAME: 'External line' },
        requestId: 'line-add',
    })
    ```

- Python

    ```python
    client.telephony.external_line.add(number="line-1", name="External line").response
    ```


- PHP

    ```php
    $b24->core->call('telephony.externalLine.add', [
        'NUMBER' => 'line-1',
        'NAME' => 'External line',
    ]);
    ```
{% endlist %}

The successful response contains the identifier of the created line.

```json
{
    "result": {
        "ID": 7
    }
}
```

If you need to handle outgoing calls from the CRM, subscribe the application to the `ONEXTERNALCALLSTART` event. Pass the public HTTPS URL of your handler in `handler`.

{% list tabs %}

- JS

    ```js
    await $b24.actions.v2.call.make({
        method: 'event.bind',
        params: {
            event: 'ONEXTERNALCALLSTART',
            handler: 'https://your-domain.example/events',
        },
        requestId: 'event-bind',
    })
    ```

- Python

    ```python
    client.event.bind(
        event="ONEXTERNALCALLSTART",
        handler="https://your-domain.example/events",
    ).response
    ```


- PHP

    ```php
    $b24->core->call('event.bind', [
        'event' => 'ONEXTERNALCALLSTART',
        'handler' => 'https://your-domain.example/events',
    ]);
    ```
{% endlist %}

Successful subscription returns `true`.

```json
{
    "result": true
}
```

## 2. Register an Incoming Call

When the PBX receives an incoming call, call [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md):

- `USER_ID` — the employee to whom the card should be displayed
- `PHONE_NUMBER` — the customer number
- `TYPE = 2` — an incoming call
- `LINE_NUMBER` — the external line number
- `EXTERNAL_CALL_ID` — the unique ID of the call on the PBX side
- `SHOW = 1` (or do not pass) — the card will open for the user from `USER_ID`

The method returns `CALL_ID` for further actions (`show`, `hide`, `finish`, `attachRecord`).

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.register',
        params: {
            USER_ID: 1269,
            PHONE_NUMBER: '499062195047',
            TYPE: 2,
            LINE_NUMBER: 'line-1',
            EXTERNAL_CALL_ID: 'asterisk-1773130778.18441',
            SHOW: 1,
        },
        requestId: 'call-register',
    })

    const callId = response.getData().result.CALL_ID
    ```

- Python

    ```python
    bitrix_response = client.telephony.external_call.register(
        phone_number="499062195047",
        call_type=2,
        user_id=1269,
        line_number="line-1",
        external_call_id="asterisk-1773130778.18441",
        show=1,
    ).response
    call_id = bitrix_response.result["CALL_ID"]
    ```


- PHP

    ```php
    $response = $b24->core->call('telephony.externalCall.register', [
        'USER_ID' => 1269,
        'PHONE_NUMBER' => '499062195047',
        'TYPE' => 2,
        'LINE_NUMBER' => 'line-1',
        'EXTERNAL_CALL_ID' => 'asterisk-1773130778.18441',
        'SHOW' => 1,
    ]);

    $callId = $response->getResponseData()->getResult()['CALL_ID'];
    ```
{% endlist %}

The successful response contains `CALL_ID`. Retain it: this identifier is required to show, hide, and finish the call and attach the recording.

```json
{
    "result": {
        "CALL_ID": "externalCall.716f1cb73def9700a23842adf9c4c568.1773130779",
        "CRM_CREATED_LEAD": null,
        "CRM_CREATED_ENTITIES": [],
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": 797
    }
}
```

## 3. Showing a Call to an Employee Group

**Simultaneous Queue.** Pass an array of employee identifiers to the `USER_ID` parameter of the [telephony.externalCall.show](../../api-reference/telephony/telephony-external-call-show.md) method. When the operator answers, hide the card for the others using the [telephony.externalCall.hide](../../api-reference/telephony/telephony-external-call-hide.md) method.

In the example, the card is shown to three employees, and then, when employee `1270` answers, it is hidden for the others.

{% list tabs %}

- JS

    ```js
    const queue = [1269, 1270, 1271]

    await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.show',
        params: { CALL_ID: callId, USER_ID: queue },
        requestId: 'call-show',
    })

    const answeredUserId = 1270
    const usersToHide = queue.filter((userId) => userId !== answeredUserId)

    await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.hide',
        params: { CALL_ID: callId, USER_ID: usersToHide },
        requestId: 'call-hide',
    })
    ```

- Python

    ```python
    queue = [1269, 1270, 1271]
    client.telephony.external_call.show(call_id=call_id, user_id=queue).response

    answered_user_id = 1270
    users_to_hide = [uid for uid in queue if uid != answered_user_id]
    client.telephony.external_call.hide(call_id=call_id, user_id=users_to_hide).response
    ```


- PHP

    ```php
    $queue = [1269, 1270, 1271];

    // Typed analog: $b24->getTelephonyScope()->externalCall()->show($callId, $queue);
    $b24->core->call('telephony.externalCall.show', [
        'CALL_ID' => $callId,
        'USER_ID' => $queue,
    ]);

    $answeredUserId = 1270;
    $usersToHide = array_values(array_filter($queue, fn($userId) => $userId !== $answeredUserId));

    $b24->core->call('telephony.externalCall.hide', [
        'CALL_ID' => $callId,
        'USER_ID' => $usersToHide,
    ]);
    ```
{% endlist %}

The `show` and `hide` methods return `true` if the command to show or hide the card was sent.

```json
{
    "result": true
}
```

**Sequential Queue.** Show the card to the first employee using method `show`. If the employee does not answer within the time specified in the PBX, hide the card using method `hide` and show it to the next employee using method `show`.

## 4. Routing a Call to the Customer's Responsible Person

To show a call to the responsible manager, first find the customer by phone number using the [telephony.externalCall.searchCrmEntities](../../api-reference/telephony/telephony-external-call-search-crm-entities.md) method. The method returns the found CRM entities and `ASSIGNED_BY_ID`, the identifier of the responsible employee.

Then register the call using `telephony.externalCall.register` with the following parameters:

- `USER_ID` — `ASSIGNED_BY_ID` from the search result
- `PHONE_NUMBER` — the customer number
- `TYPE = 2` — an incoming call
- `LINE_NUMBER` — the external line number
- `EXTERNAL_CALL_ID` — the unique call identifier on the PBX side
- `SHOW = 0` — do not show the card immediately after registration

After registration, pass `CALL_ID` and `ASSIGNED_BY_ID` to `telephony.externalCall.show`.

{% list tabs %}

- JS

    ```js
    const crmSearch = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.searchCrmEntities',
        params: { PHONE_NUMBER: '499062195047' },
        requestId: 'crm-search',
    })

    const crmEntities = crmSearch.getData().result
    if (!crmEntities.length) {
        throw new Error('Customer with this phone number was not found in the CRM')
    }

    const assignedById = Number(crmEntities[0].ASSIGNED_BY_ID)

    const reg = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.register',
        params: {
            USER_ID: assignedById,
            PHONE_NUMBER: '499062195047',
            TYPE: 2,
            LINE_NUMBER: 'line-1',
            EXTERNAL_CALL_ID: 'asterisk-1773130778.18441-manager',
            SHOW: 0,
        },
        requestId: 'call-register',
    })

    const callId = reg.getData().result.CALL_ID

    await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.show',
        params: { CALL_ID: callId, USER_ID: assignedById },
        requestId: 'call-show',
    })
    ```

- Python

    ```python
    crm_entities = client.telephony.external_call.search_crm_entities(
        phone_number="499062195047",
    ).response.result

    if not crm_entities:
        raise RuntimeError("Customer with this phone number was not found in the CRM")

    assigned_by_id = int(crm_entities[0]["ASSIGNED_BY_ID"])

    reg = client.telephony.external_call.register(
        phone_number="499062195047",
        call_type=2,
        user_id=assigned_by_id,
        line_number="line-1",
        external_call_id="asterisk-1773130778.18441-manager",
        show=0,
    ).response.result

    client.telephony.external_call.show(
        call_id=reg["CALL_ID"],
        user_id=assigned_by_id,
    ).response
    ```


- PHP

    ```php
    $crmEntities = $b24->core->call('telephony.externalCall.searchCrmEntities', [
        'PHONE_NUMBER' => '499062195047',
    ])->getResponseData()->getResult();

    if (empty($crmEntities)) {
        throw new \RuntimeException('Customer with this phone number was not found in the CRM');
    }

    $assignedById = (int)$crmEntities[0]['ASSIGNED_BY_ID'];

    $reg = $b24->core->call('telephony.externalCall.register', [
        'USER_ID' => $assignedById,
        'PHONE_NUMBER' => '499062195047',
        'TYPE' => 2,
        'LINE_NUMBER' => 'line-1',
        'EXTERNAL_CALL_ID' => 'asterisk-1773130778.18441-manager',
        'SHOW' => 0,
    ])->getResponseData()->getResult();

    $b24->core->call('telephony.externalCall.show', [
        'CALL_ID' => $reg['CALL_ID'],
        'USER_ID' => $assignedById,
    ]);
    ```
{% endlist %}

Store `CALL_ID` from the `register` response. You need it to finish the call and attach the recording.

## 5. Handling an Outgoing Call from the CRM

When an employee clicks on a number in the CRM, Bitrix24 registers the call and sends the [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) event to the application with fields `CALL_ID`, `PHONE_NUMBER`, `USER_ID`, `LINE_NUMBER`, `CRM_ENTITY_TYPE`, `CRM_ENTITY_ID`, and `CALL_LIST_ID`.

Your web server receives the event. After initiating the call on the PBX, complete the same `CALL_ID` using the `finish` method. The `CALL_ID` field arrives in the event, so you do not need to call `register` again for an outgoing call.

{% list tabs %}

- JS

    ```js
    import express from 'express'
    const app = express()
    app.use(express.urlencoded({ extended: true }))

    app.post('/events', async (req, res) => {
        if (req.body.event === 'ONEXTERNALCALLSTART') {
            const data = req.body.data
            // ... initiate call to PBX via data.PHONE_NUMBER ...
            // upon completion of the call:
            await $b24.actions.v2.call.make({
                method: 'telephony.externalCall.finish',
                params: { CALL_ID: data.CALL_ID, USER_ID: data.USER_ID, DURATION: 95, STATUS_CODE: '200' },
                requestId: 'call-finish',
            })
        }
        res.send('ok')
    })
    ```

- Python

    ```python
    from flask import Flask, request
    app = Flask(__name__)

    @app.post("/events")
    def events():
        if request.form.get("event") == "ONEXTERNALCALLSTART":
            data = request.form  # fields arrive as data[CALL_ID] etc..
            # ... initiate call to PBX ...
            client.telephony.external_call.finish(
                call_id=data.get("data[CALL_ID]"),
                user_id=int(data.get("data[USER_ID]")),
                duration=95,
                status_code="200",
            ).response
        return "ok"
    ```


- PHP

    ```php
    <?php
    // ONEXTERNALCALLSTART event handler
    if (($_REQUEST['event'] ?? '') === 'ONEXTERNALCALLSTART') {
        $data = $_REQUEST['data'];
        // ... initiate call to PBX via $data['PHONE_NUMBER'] ...
        $b24->core->call('telephony.externalCall.finish', [
            'CALL_ID' => $data['CALL_ID'],
            'USER_ID' => $data['USER_ID'],
            'DURATION' => 95,
            'STATUS_CODE' => '200',
        ]);
    }
    ```
{% endlist %}

The handler must return the HTTP response `200`. After that, Bitrix24 treats the event as delivered.

## 6. Finishing a Call and Retaining the Result

After the conversation, call [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md): the method hides the card, retains the call in statistics, and creates a CRM activity. Pass `CALL_ID`, `USER_ID`, `DURATION` (sec), and `STATUS_CODE` (`200` — successful, `304` — missed).

If the recording is not yet ready, call `finish` without a recording, and attach it later using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method. Once the recording is available, you can add a transcription using the [telephony.call.attachTranscription](../../api-reference/telephony/telephony-call-attach-transcription.md) method.

{% list tabs %}

- JS

    ```js
    const finishResponse = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.finish',
        params: { CALL_ID: callId, USER_ID: 1270, DURATION: 95, STATUS_CODE: '200', ADD_TO_CHAT: 1 },
        requestId: 'call-finish',
    })

    // later, when the recording is ready
    await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.attachRecord',
        params: { CALL_ID: callId, FILENAME: 'record.mp3', RECORD_URL: 'https://your-domain.example/record.mp3' },
        requestId: 'attach-record',
    })
    ```

- Python

    ```python
    finish_response = client.telephony.external_call.finish(
        call_id=call_id, user_id=1270, duration=95, status_code="200", add_to_chat=1,
    ).response

    # later, when the recording is ready
    client.telephony.external_call.attach_record(
        call_id=call_id, filename="record.mp3", record_url="https://your-domain.example/record.mp3",
    ).response
    ```


- PHP

    ```php
    $finishResponse = $b24->core->call('telephony.externalCall.finish', [
        'CALL_ID' => $callId, 'USER_ID' => 1270, 'DURATION' => 95, 'STATUS_CODE' => '200', 'ADD_TO_CHAT' => 1,
    ]);

    // later, when the recording is ready
    $b24->core->call('telephony.externalCall.attachRecord', [
        'CALL_ID' => $callId, 'FILENAME' => 'record.mp3', 'RECORD_URL' => 'https://your-domain.example/record.mp3',
    ]);
    ```
{% endlist %}

The successful `finish` response contains the call statistics record. The `CALL_STATUS`, `CALL_FAILED_CODE`, `CRM_ACTIVITY_ID`, `CRM_ENTITY_TYPE`, and `CRM_ENTITY_ID` fields help verify that the call is finished and linked to the CRM.

```json
{
    "result": {
        "CALL_ID": "externalCall.716f1cb73def9700a23842adf9c4c568.1773130779",
        "PORTAL_USER_ID": 1270,
        "PHONE_NUMBER": "499062195047",
        "CALL_DURATION": 95,
        "CALL_STATUS": 1,
        "CALL_FAILED_CODE": "200",
        "CRM_ACTIVITY_ID": 7943,
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": 797,
        "ID": 7
    }
}
```

If the recording is attached by `RECORD_URL`, the `attachRecord` method returns the file identifier.

```json
{
    "result": {
        "FILE_ID": 9079
    }
}
```

## Recording a Call Without Showing a Card

If the connection between the PBX and Bitrix24 was unavailable, record the call without a card after connectivity is restored: call `register` with `SHOW = 0`, then `finish` with the actual data. The scenario does not show the call in real time, but it retains the history, statistics, and CRM activity.

{% list tabs %}

- JS

    ```js
    const reg = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.register',
        params: {
            USER_ID: 1269,
            PHONE_NUMBER: '499062195047',
            TYPE: 2,
            LINE_NUMBER: 'line-1',
            EXTERNAL_CALL_ID: 'asterisk-1773130778.18441-offline',
            SHOW: 0,
        },
        requestId: 'call-register',
    })
    const callId = reg.getData().result.CALL_ID

    await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.finish',
        params: { CALL_ID: callId, USER_ID: 1269, DURATION: 0, STATUS_CODE: '304' },
        requestId: 'call-finish',
    })
    ```

- Python

    ```python
    call_id = client.telephony.external_call.register(
        phone_number="499062195047",
        call_type=2,
        user_id=1269,
        line_number="line-1",
        external_call_id="asterisk-1773130778.18441-offline",
        show=0,
    ).response.result["CALL_ID"]

    client.telephony.external_call.finish(
        call_id=call_id, user_id=1269, duration=0, status_code="304",
    ).response
    ```


- PHP

    ```php
    $callId = $b24->core->call('telephony.externalCall.register', [
        'USER_ID' => 1269,
        'PHONE_NUMBER' => '499062195047',
        'TYPE' => 2,
        'LINE_NUMBER' => 'line-1',
        'EXTERNAL_CALL_ID' => 'asterisk-1773130778.18441-offline',
        'SHOW' => 0,
    ])->getResponseData()->getResult()['CALL_ID'];

    $b24->core->call('telephony.externalCall.finish', [
        'CALL_ID' => $callId, 'USER_ID' => 1269, 'DURATION' => 0, 'STATUS_CODE' => '304',
    ]);
    ```
{% endlist %}

## Check the Result

Verify that the integration processed the call:

- during an incoming call, the call card opened for the required employee
- after the operator answered, the card was hidden for the remaining employees in the queue
- after `finish`, the call appeared in telephony statistics
- a call activity was created in the CRM if the call is linked to a contact, company, lead, or deal
- after `attachRecord`, the call recording is available in the call activity

Through REST, the successful result is confirmed by the `finish` response fields: `ID`, `CALL_STATUS`, `CALL_FAILED_CODE`, `CRM_ACTIVITY_ID`, `CRM_ENTITY_TYPE`, and `CRM_ENTITY_ID`.

Output these fields from the `finish` response received in step 6.

{% list tabs %}

- JS

    ```js
    const finishResult = finishResponse.getData().result

    console.table({
        ID: finishResult.ID,
        CALL_STATUS: finishResult.CALL_STATUS,
        CALL_FAILED_CODE: finishResult.CALL_FAILED_CODE,
        CRM_ACTIVITY_ID: finishResult.CRM_ACTIVITY_ID,
        CRM_ENTITY_TYPE: finishResult.CRM_ENTITY_TYPE,
        CRM_ENTITY_ID: finishResult.CRM_ENTITY_ID,
    })
    ```

- Python

    ```python
    finish_result = finish_response.result

    for field in [
        "ID",
        "CALL_STATUS",
        "CALL_FAILED_CODE",
        "CRM_ACTIVITY_ID",
        "CRM_ENTITY_TYPE",
        "CRM_ENTITY_ID",
    ]:
        print(field, finish_result.get(field))
    ```


- PHP

    ```php
    $finishResult = $finishResponse->getResponseData()->getResult();

    echo 'ID: ' . $finishResult['ID'] . PHP_EOL;
    echo 'CALL_STATUS: ' . $finishResult['CALL_STATUS'] . PHP_EOL;
    echo 'CALL_FAILED_CODE: ' . $finishResult['CALL_FAILED_CODE'] . PHP_EOL;
    echo 'CRM_ACTIVITY_ID: ' . $finishResult['CRM_ACTIVITY_ID'] . PHP_EOL;
    echo 'CRM_ENTITY_TYPE: ' . $finishResult['CRM_ENTITY_TYPE'] . PHP_EOL;
    echo 'CRM_ENTITY_ID: ' . $finishResult['CRM_ENTITY_ID'] . PHP_EOL;
    ```
{% endlist %}

## Errors and Diagnostics

If the method returns an error, check the request data and the authorization context.

#|
|| **Error Code or Message** | **Cause and action** ||
|| `WRONG_AUTH_TYPE` | The method was called outside the application context. Verify the application's OAuth authorization and the `telephony` scope ||
|| `USER_ID or USER_PHONE_INNER should be set` | No employee was passed to `register` or `finish`. Pass an active `USER_ID` or the employee's internal number ||
|| `Unknown TYPE` | An invalid call type was passed to `register`. For an incoming call, use `TYPE = 2` ||
|| `Unsupported phone number format` | The customer number was not recognized. Pass the phone number in international format without letters ||
|| `Line already exists` | A line with this `NUMBER` is already registered. Use an existing `LINE_NUMBER` or change the line number ||
|| `CALL_ID must be a string` | An invalid `CALL_ID` type was passed to `finish`. Pass the string returned by `register` or the `ONEXTERNALCALLSTART` event ||
|| `Call is not found` | The call was not found or is already finished. Verify that `CALL_ID` was stored from the current call ||
|| `Call is not found in the statistic table. Looks like it is not finished yet.` | The recording is being attached before the call is finished. Call `finish` first, then `attachRecord` ||
|| `Required parameters are not set. Request should contain or URL or FILENAME parameter` | `RECORD_URL` or `FILENAME` was not passed to `attachRecord` ||
|| `Wrong file extension. Only wav and mp3 are allowed` | The recording was passed in an unsupported format. Use a `wav` or `mp3` file ||
|| `ERROR_EVENT_NOT_FOUND` | An invalid event was passed to `event.bind`. Pass `ONEXTERNALCALLSTART` ||
|#

Repeat the scenario from the step where the error occurred. If the error occurs after a successful `register`, keep using the same `CALL_ID` until the call is finished.

## What to Consider

- Outgoing calls from the CRM require an installed `ONEXTERNALCALLSTART` handler. An incoming webhook does not receive this event
- Pass a unique `EXTERNAL_CALL_ID` for each physical call so that a repeated `register` call does not return an existing `CALL_ID`
- Store the application's OAuth tokens on the server and do not expose them in public client-side code
- The event handler must be reachable over HTTPS and accept POST requests from Bitrix24
- Attach the call recording after `finish`, when the call is already saved in statistics

## Continue Learning

- [Telephony Methods Overview](../../api-reference/telephony/index.md)
- [Telephony Events](../../api-reference/telephony/events/index.md)
- [Tab in the call record CALL_CARD](../../api-reference/widgets/telephony/call-card.md)
