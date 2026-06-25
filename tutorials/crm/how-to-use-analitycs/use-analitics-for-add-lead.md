# How to Use Sales Intelligence When Creating a Lead

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: a user with permission to add a lead. To link a trace, permissions to edit a lead are required.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sales Intelligence shows the customer's acquisition source. When a customer fills out a form on a site, you can pass the name, phone number, and advertising channel data with the visit path to the lead card.

Sales Intelligence creates a tracker on the site. The tracker collects visit data. When a form is submitted, the code receives this data and links the lead to the customer's acquisition source.

Setting up data transmission consists of four stages.

1. Add a Feedback form and a hidden field `TRACE` to the page.
2. Retrieve the visitor's trace via `b24Tracker.guest.getTrace()` and save the visit identifier in the form's hidden field.
3. Create a lead using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method.
4. Link the lead to the trace using the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method.

## 1. Add a Form to the Site

Add fields to the Feedback form:

- `NAME` — customer name,
- `LAST_NAME` — customer last name,
- `PHONE` — customer phone number,
- `TRACE` — Sales Intelligence data, a hidden form field.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```html
    <form id="feedbackForm">
        <input type="hidden" id="FORM_TRACE" name="TRACE">
        <input type="text" name="NAME" required>
        <input type="text" name="LAST_NAME" required>
        <input type="text" name="PHONE" required>
        <input type="submit" name="SAVE" value="Send">
    </form>
    ```

- PHP

    ```html
    <form method="post" action="">
        <input type="hidden" id="FORM_TRACE" name="TRACE">
        <input type="text" name="NAME" required>
        <input type="text" name="LAST_NAME" required>
        <input type="text" name="PHONE" required>
        <input type="submit" name="SAVE" value="Send">
    </form>
    ```

{% endlist %}

The user does not see the hidden field, but its value is sent along with the rest of the form data.

## 2. Retrieve Sales Intelligence Data

After the page loads, access the `b24Tracker` object to retrieve the current visitor's trace. Write the value to the `TRACE` hidden field.

{% list tabs %}

- JS

    ```js
    window.onload = function(e){
        var traceInput = document.getElementById('FORM_TRACE');
        if(
            traceInput
            && typeof b24Tracker !== 'undefined'
            && b24Tracker.guest
            && typeof b24Tracker.guest.getTrace === 'function'
        )
        {
            traceInput.value = b24Tracker.guest.getTrace();
        }
    }
    ```

- PHP

    ```html
    <script>
        window.onload = function(e){
            var traceInput = document.getElementById('FORM_TRACE');
            if(
                traceInput
                && typeof b24Tracker !== 'undefined'
                && b24Tracker.guest
                && typeof b24Tracker.guest.getTrace === 'function'
            )
            {
                traceInput.value = b24Tracker.guest.getTrace();
            }
        }
    </script>
    ```

{% endlist %}

The retrieved value is used to link the lead to an Ad source and is displayed in Sales Intelligence reports.

{% note warning "" %}

If the Sales Intelligence script is not installed on the site or fails to load before the `b24Tracker.guest.getTrace()` call, value `TRACE` will not be retrieved. Verify the script connection on the page containing the form.

{% endnote %}

## 3. Create a Lead

To create a lead, use the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. Pass the value `1` — the lead object type identifier — in the `entityTypeId` parameter.

Pass the following parameters in `fields`:

- `title` — lead name,
- `name` — customer name,
- `lastName` — customer last name,
- `fm` — phone number in the CRM multi-field format.

Pass the `fm` field as an array because the phone number in the CRM is stored as a [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) type multi-field. For the phone number, specify:

- `typeId` — `PHONE` multi-field type,
- `valueType` — value type, for example `WORK`,
- `value` — phone number.

{% note warning "" %}

Check which mandatory fields are configured for leads in your Bitrix24. All mandatory fields must be passed to the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```js
    var formData = new FormData(event.target);
    var fields = {
        title: 'Feedback page: ' + formData.get('NAME') + ' ' + formData.get('LAST_NAME'),
        name: formData.get('NAME'),
        lastName: formData.get('LAST_NAME'),
        fm: [
            {
                typeId: 'PHONE',
                valueType: 'WORK',
                value: formData.get('PHONE')
            }
        ]
    };

    BX24.callMethod(
        'crm.item.add',
        {
            entityTypeId: 1,
            fields: fields
        },
        function(resultLead) {
            if(resultLead.error()) {
                console.log(resultLead.error_description());
            } else {
                var leadId = resultLead.data().item.id;
            }
        }
    );
    ```

- PHP

    ```php
    $name = htmlspecialchars($_POST['NAME'] ?? '');
    $lastName = htmlspecialchars($_POST['LAST_NAME'] ?? '');
    $phone = htmlspecialchars($_POST['PHONE'] ?? '');
    $fields = [
        'title' => 'Feedback page: '.$name.' '.$lastName,
        'name' => $name,
        'lastName' => $lastName,
        'fm' => [
            [
                'typeId' => 'PHONE',
                'valueType' => 'WORK',
                'value' => $phone
            ]
        ],
    ];

    $result = CRest::call(
        'crm.item.add',
        [
            'entityTypeId' => 1,
            'fields' => $fields
        ]
    );

    $leadId = $result['result']['item']['id'];
    ```

{% endlist %}

The [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method returns the lead identifier in the `result.item.id` field.

An abbreviated example of the response is shown below. For the full response format, see the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method description.

```json
{
    "result": {
        "item": {
            "id": 123
        }
    }
}
```

## 4. Linking a Lead to a Trace

After creating a lead, call the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method because `TRACE` cannot be passed directly to [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md).

You can pass UTM parameters to [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md): `utmSource`, `utmMedium`, `utmCampaign`, `utmContent`, `utmTerm`. These retain advertising tags in the lead but do not replace the full trace.

Pass the following parameters to the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method:

- `TRACE` — a string containing Sales Intelligence data.
- `ENTITIES` — an array of objects to be linked to the trace. For a lead, specify `TYPE` with the value `LEAD` and `ID` from the `result.item.id` field of the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) response.

{% list tabs %}

- JS

    ```js
    var trace = formData.get('TRACE');

    if(trace) {
        BX24.callMethod(
            'crm.tracking.trace.add',
            {
                TRACE: trace,
                ENTITIES: [
                    {
                        TYPE: 'LEAD',
                        ID: leadId
                    }
                ]
            },
            function(resultTrace) {
                if(resultTrace.error()) {
                    console.log(resultTrace.error_description());
                }
            }
        );
    }
    ```

- PHP

    ```php
    $trace = $_POST['TRACE'] ?? '';

    if(!empty($trace))
    {
        $resultTrace = CRest::call(
            'crm.tracking.trace.add',
            [
                'TRACE' => $trace,
                'ENTITIES' => [
                    [
                        'TYPE' => 'LEAD',
                        'ID' => $leadId
                    ]
                ]
            ]
        );
    }
    ```

{% endlist %}

If `TRACE` is empty, the lead will be created without a link to Sales Intelligence.

The [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method returns the identifier of the created trace in the `result` field.

```json
{
    "result": 341
}
```

### Full Code Example

{% list tabs %}

- JS

    ```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        </head>
        <body class="container">
            <h1>Feedback</h1>
            <div class="col-12">
                <p id="message"></p>
            </div>
            <form id="feedbackForm">
                <input type="hidden" id="FORM_TRACE" name="TRACE">
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Name*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="NAME" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Last name*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="LAST_NAME" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Phone*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="PHONE" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-sm-10">
                        <input type="submit" name="SAVE" class="btn btn-primary" value="Send">
                    </div>
                </div>
            </form>
            <!-- The Sales Intelligence script must be installed on the page -->
            <script>
                window.onload = function(e){
                    var traceInput = document.getElementById('FORM_TRACE');
                    if(
                        traceInput
                        && typeof b24Tracker !== 'undefined'
                        && b24Tracker.guest
                        && typeof b24Tracker.guest.getTrace === 'function'
                    )
                    {
                        traceInput.value = b24Tracker.guest.getTrace();
                    }
                }

                document.getElementById('feedbackForm').addEventListener('submit', function(event) {
                    event.preventDefault();
                    var formData = new FormData(event.target);
                    var fields = {
                        title: 'Feedback page: ' + formData.get('NAME') + ' ' + formData.get('LAST_NAME'),
                        name: formData.get('NAME'),
                        lastName: formData.get('LAST_NAME'),
                        fm: [
                            {
                                typeId: 'PHONE',
                                valueType: 'WORK',
                                value: formData.get('PHONE')
                            }
                        ]
                    };

                    BX24.callMethod(
                        'crm.item.add',
                        {
                            entityTypeId: 1,
                            fields: fields
                        },
                        function(resultLead) {
                            var messageElement = document.getElementById('message');
                            if(resultLead.error()) {
                                messageElement.textContent = 'Lead not created: ' + resultLead.error_description();
                            } else {
                                var leadId = resultLead.data().item.id;
                                var trace = formData.get('TRACE');

                                if(trace) {
                                    BX24.callMethod(
                                        'crm.tracking.trace.add',
                                        {
                                            TRACE: trace,
                                            ENTITIES: [
                                                {
                                                    TYPE: 'LEAD',
                                                    ID: leadId
                                                }
                                            ]
                                        },
                                        function(resultTrace) {
                                            if(resultTrace.error()) {
                                                messageElement.textContent = 'Lead created, but trace not saved: ' + resultTrace.error_description();
                                            } else {
                                                messageElement.textContent = 'Lead created';
                                            }
                                        }
                                    );
                                } else {
                                    messageElement.textContent = 'Lead created without trace';
                                }
                            }
                        }
                    );
                });
            </script>
        </body>
    </html>
    ```

- PHP

    ```php
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        </head>
        <body class="container">
            <h1>Feedback</h1>
            <?php
            include("crest.php");
            $message = '';
            if(!empty($_POST['SAVE']))
            {
                $name = htmlspecialchars($_POST['NAME'] ?? '');
                $lastName = htmlspecialchars($_POST['LAST_NAME'] ?? '');
                $phone = htmlspecialchars($_POST['PHONE'] ?? '');
                $trace = $_POST['TRACE'] ?? '';
                $fields = [
                    'title' => 'Feedback page: '.$name.' '.$lastName,
                    'name' => $name,
                    'lastName' => $lastName,
                    'fm' => [
                        [
                            'typeId' => 'PHONE',
                            'valueType' => 'WORK',
                            'value' => $phone
                        ]
                    ],
                ];
                $result = CRest::call(
                    'crm.item.add',
                    [
                        'entityTypeId' => 1,
                        'fields' => $fields
                    ]
                );
                if (!empty($result['result']['item']['id']))
                {
                    $leadId = $result['result']['item']['id'];
                    if(!empty($trace))
                    {
                        $resultTrace = CRest::call(
                            'crm.tracking.trace.add',
                            [
                                'TRACE' => $trace,
                                'ENTITIES' => [
                                    [
                                        'TYPE' => 'LEAD',
                                        'ID' => $leadId
                                    ]
                                ]
                            ]
                        );
                        if (!empty($resultTrace['error_description']))
                        {
                            $message = 'Lead created, but trace not saved: '.$resultTrace['error_description'];
                        }
                        else
                        {
                            $message = 'Lead created';
                        }
                    }
                    else
                    {
                        $message = 'Lead created without trace';
                    }
                }
                elseif (!empty($result['error_description']))
                {
                    $message =    'Lead not created: '.$result['error_description'];
                }
                else
                {
                    $message = 'Lead not created';
                }
            }
            ?>
            <div class="col-12">
                <p><?=$message?></p>
            </div>
            <form method="post" action="">
                <input type="hidden" id="FORM_TRACE" name="TRACE">
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Name*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="NAME" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Last name*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="LAST_NAME" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-4 mt-3">
                        <label>Phone*</label>
                    </div>
                    <div class="col-6 mt-3">
                        <input type="text" name="PHONE" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-sm-10">
                        <input type="submit" name="SAVE" class="btn btn-primary" value="Send">
                    </div>
                </div>
            </form>
            <!-- The Sales Intelligence script must be installed on the page -->
            <script>
                window.onload = function(e){
                    var traceInput = document.getElementById('FORM_TRACE');
                    if(
                        traceInput
                        && typeof b24Tracker !== 'undefined'
                        && b24Tracker.guest
                        && typeof b24Tracker.guest.getTrace === 'function'
                    )
                    {
                        traceInput.value = b24Tracker.guest.getTrace();
                    }
                }
            </script>
        </body>
    </html>
    ```

{% endlist %}

## Verifying the Result

After submitting the form, a new lead will appear in the CRM with the customer's first name, last name, and phone number. If the `TRACE` field is populated, the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method will link the lead to the Sales Intelligence data.

## Continue Learning

- [{#T}](./info-to-analitics.md)
- [{#T}](./use-analitics-for-add-contact.md)
- [{#T}](../../../api-reference/crm/tracking/crm-tracking-trace-add.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-add.md)