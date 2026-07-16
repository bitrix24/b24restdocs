# How to Integrate External Telephony with Bitrix24

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

External telephony transmits call data from the PBX to Bitrix24: client number, user, line, call status, and recording. Bitrix24 displays the call detail form to the employee, links the call to the CRM, and saves the result in the statistics.

To integrate external telephony, follow these six steps:

1. Assemble the application and handlers for the PBX and Bitrix24
2. Register an incoming call
3. Display the call card to an employee group
4. Route the call to the customer's responsible person
5. Process an outgoing call from the CRM
6. Complete the call and save the result

We will separately examine a scenario where a call must be recorded without displaying a card.

{% note info "" %}

REST methods `telephony.externalCall.*` and `telephony.externalLine.*` work both via an incoming webhook and within the context of an application. The [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) event (step 5) is sent only to an installed application—it is received by your web server.

In PHP, telephony methods are called directly through the core (`$b24->core->call(...)`). Typed analogs are available in `getTelephonyScope()->externalCall()` (`show`, `hide`, `register`, `finishForUserId`) and `->externalLine()`, but they require value objects (`CallType`, `TelephonyCallStatusCode`, `Money`, `CarbonImmutable`).

{% endnote %}

## SDK Initialization

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/');
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import Client, BitrixWebhook

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="1/xxxxxxxxxxxxxxxx",
    ))
    ```

{% endlist %}

## 1. Assemble the Application

A working integration typically consists of a server-side application and handlers for the PBX and Bitrix24:

1. Create a [local application](../../settings/app-installation/local-apps/index.md) or a Market application
2. Complete the application installation and retain the authorization
3. Register an external line using the [telephony.externalLine.add](../../api-reference/telephony/telephony-external-line-add.md) method. Pass the line number in `LINE_NUMBER` of the [telephony.externalCall.register](../../api-reference/telephony/telephony-external-call-register.md) method
4. Subscribe the application to [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) using the [event.bind](../../api-reference/events/event-bind.md) method if you need to initiate outgoing calls from the CRM
5. Create an event handler from the PBX that calls `telephony.externalCall.register/show/hide/finish` based on the call status
6. Create an [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) handler for outgoing calls
7. If the call recording appears after completion, attach it using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method

Registering an external line:

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'telephony.externalLine.add',
        params: { NUMBER: 'line-1', NAME: 'External line' },
        requestId: 'line-add',
    })
    ```

- PHP

    ```php
    $b24->core->call('telephony.externalLine.add', [
        'NUMBER' => 'line-1',
        'NAME' => 'External line',
    ]);
    ```

- Python

    ```python
    client.telephony.external_line.add(number="line-1", name="External line").response
    ```

{% endlist %}

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

{% endlist %}

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

- Python

    ```python
    queue = [1269, 1270, 1271]
    client.telephony.external_call.show(call_id=call_id, user_id=queue).response

    answered_user_id = 1270
    users_to_hide = [uid for uid in queue if uid != answered_user_id]
    client.telephony.external_call.hide(call_id=call_id, user_id=users_to_hide).response
    ```

{% endlist %}

**Sequential Queue.** Show the card to the first employee using method `show`. If he does not answer within the time specified in the PBX, hide the card using method `hide` and show it to the next employee using method `show`.

## 4. Routing a Call to the Customer's Responsible Person

To show a call to the responsible manager, register the call with `SHOW = 0`. Bitrix24 will find the CRM object by number and return `CRM_ENTITY_TYPE` and `CRM_ENTITY_ID`. Get the responsible person from the object and pass it to `telephony.externalCall.show`.

{% list tabs %}

- JS

    ```js
    const reg = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.register',
        params: { PHONE_NUMBER: '499062195047', TYPE: 2, LINE_NUMBER: 'line-1', SHOW: 0 },
        requestId: 'call-register',
    })
    const { CALL_ID, CRM_ENTITY_TYPE, CRM_ENTITY_ID } = reg.getData().result

    let assignedById
    if (CRM_ENTITY_TYPE === 'CONTACT' && CRM_ENTITY_ID) {
        const contact = await $b24.actions.v2.call.make({
            method: 'crm.contact.get', params: { id: CRM_ENTITY_ID }, requestId: 'contact-get',
        })
        assignedById = contact.getData().result.ASSIGNED_BY_ID
    }

    if (assignedById) {
        await $b24.actions.v2.call.make({
            method: 'telephony.externalCall.show',
            params: { CALL_ID, USER_ID: assignedById },
            requestId: 'call-show',
        })
    }
    ```

- PHP

    ```php
    $reg = $b24->core->call('telephony.externalCall.register', [
        'PHONE_NUMBER' => '499062195047', 'TYPE' => 2, 'LINE_NUMBER' => 'line-1', 'SHOW' => 0,
    ])->getResponseData()->getResult();

    $assignedById = null;
    if (($reg['CRM_ENTITY_TYPE'] ?? '') === 'CONTACT' && !empty($reg['CRM_ENTITY_ID'])) {
        $contact = $b24->getCRMScope()->contact()->get((int)$reg['CRM_ENTITY_ID'])->contact();
        $assignedById = $contact->ASSIGNED_BY_ID;
    }

    if ($assignedById) {
        $b24->core->call('telephony.externalCall.show', [
            'CALL_ID' => $reg['CALL_ID'],
            'USER_ID' => [$assignedById],
        ]);
    }
    ```

- Python

    ```python
    reg = client.telephony.external_call.register(
        phone_number="499062195047", call_type=2, line_number="line-1", show=0,
    ).response.result

    assigned_by_id = None
    if reg.get("CRM_ENTITY_TYPE") == "CONTACT" and reg.get("CRM_ENTITY_ID"):
        contact = client.crm.contact.get(bitrix_id=reg["CRM_ENTITY_ID"]).response.result
        assigned_by_id = contact["ASSIGNED_BY_ID"]

    if assigned_by_id:
        client.telephony.external_call.show(call_id=reg["CALL_ID"], user_id=assigned_by_id).response
    ```

{% endlist %}

To find a customer by phone number without registering a call, use [telephony.externalCall.searchCrmEntities](../../api-reference/telephony/telephony-external-call-search-crm-entities.md).

## 5. Handling an Outgoing Call from the CRM

When an employee clicks on a number in the CRM, Bitrix24 registers the call and sends the [ONEXTERNALCALLSTART](../../api-reference/telephony/events/on-external-call-start.md) event to the application with fields `CALL_ID`, `PHONE_NUMBER`, `USER_ID`, `LINE_NUMBER`, `CRM_ENTITY_TYPE`, `CRM_ENTITY_ID`, and `CALL_LIST_ID`.

Your web server receives the event (the SDK only performs outgoing calls). After initiating the call on the PBX, complete the same `CALL_ID` using method `finish`.

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

{% endlist %}

## 6. Finishing a Call and Retaining the Result

After the conversation, call [telephony.externalCall.finish](../../api-reference/telephony/telephony-external-call-finish.md): the method hides the card, retains the call in statistics, and creates a CRM activity. Pass `CALL_ID`, `USER_ID`, `DURATION` (sec), and `STATUS_CODE` (`200` — successful, `304` — missed).

If the recording is not yet ready, call `finish` without a recording, and attach it later using the [telephony.externalCall.attachRecord](../../api-reference/telephony/telephony-external-call-attach-record.md) method. Once the recording is available, you can add a transcription using the [telephony.call.attachTranscription](../../api-reference/telephony/telephony-call-attach-transcription.md) method.

{% list tabs %}

- JS

    ```js
    await $b24.actions.v2.call.make({
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

- PHP

    ```php
    $b24->core->call('telephony.externalCall.finish', [
        'CALL_ID' => $callId, 'USER_ID' => 1270, 'DURATION' => 95, 'STATUS_CODE' => '200', 'ADD_TO_CHAT' => 1,
    ]);

    // later, when the recording is ready
    $b24->core->call('telephony.externalCall.attachRecord', [
        'CALL_ID' => $callId, 'FILENAME' => 'record.mp3', 'RECORD_URL' => 'https://your-domain.example/record.mp3',
    ]);
    ```

- Python

    ```python
    client.telephony.external_call.finish(
        call_id=call_id, user_id=1270, duration=95, status_code="200", add_to_chat=1,
    ).response

    # later, when the recording is ready
    client.telephony.external_call.attach_record(
        call_id=call_id, filename="record.mp3", record_url="https://your-domain.example/record.mp3",
    ).response
    ```

{% endlist %}

## Recording a Call Without Showing a Card

If the connection between the PBX and Bitrix24 was unavailable, save the fact of the call without a card after connectivity is restored: call `register` with `SHOW = 0`, then `finish` with the actual data. The scenario does not show the call in real time, but it retains the history, statistics, and CRM activity.

{% list tabs %}

- JS

    ```js
    const reg = await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.register',
        params: { USER_ID: 1269, PHONE_NUMBER: '499062195047', TYPE: 2, LINE_NUMBER: 'line-1', SHOW: 0 },
        requestId: 'call-register',
    })
    const callId = reg.getData().result.CALL_ID

    await $b24.actions.v2.call.make({
        method: 'telephony.externalCall.finish',
        params: { CALL_ID: callId, USER_ID: 1269, DURATION: 0, STATUS_CODE: '304' },
        requestId: 'call-finish',
    })
    ```

- PHP

    ```php
    $callId = $b24->core->call('telephony.externalCall.register', [
        'USER_ID' => 1269, 'PHONE_NUMBER' => '499062195047', 'TYPE' => 2, 'LINE_NUMBER' => 'line-1', 'SHOW' => 0,
    ])->getResponseData()->getResult()['CALL_ID'];

    $b24->core->call('telephony.externalCall.finish', [
        'CALL_ID' => $callId, 'USER_ID' => 1269, 'DURATION' => 0, 'STATUS_CODE' => '304',
    ]);
    ```

- Python

    ```python
    call_id = client.telephony.external_call.register(
        phone_number="499062195047", call_type=2, user_id=1269, line_number="line-1", show=0,
    ).response.result["CALL_ID"]

    client.telephony.external_call.finish(
        call_id=call_id, user_id=1269, duration=0, status_code="304",
    ).response
    ```

{% endlist %}

## Continue Learning

- [Telephony Methods Overview](../../api-reference/telephony/index.md)
- [Telephony Events](../../api-reference/telephony/events/index.md)
- [Tab in the call record CALL_CARD](../../api-reference/widgets/telephony/index.md)
