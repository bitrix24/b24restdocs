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

{% note info "" %}

The form on the website is public, so REST calls are performed on the server side rather than in the browser: a webhook with CRM permissions must not be exposed in client-side code. The browser only collects the form data and the trace, then sends them to your backend via a standard POST request. The backend calls Bitrix24 methods via an SDK:

- PHP — [B24PhpSDK](https://github.com/bitrix24/b24phpsdk)
- Python — [b24pysdk](https://github.com/bitrix24/b24pysdk)
- JS — [b24jssdk](https://github.com/bitrix24/b24jssdk) on the server (Node.js) via `B24Hook`

{% endnote %}

## 1. Add a Form to the Website

Add fields to the feedback form:

- `NAME` — customer name,
- `LAST_NAME` — customer surname,
- `PHONE` — customer phone number,
- `TRACE` — Sales Intelligence data, a hidden form field.

The form sends data to the backend via a standard POST request, so its markup is the same for all languages:

```html
<form method="post" action="/">
    <input type="hidden" id="FORM_TRACE" name="TRACE">
    <input type="text" name="NAME" required>
    <input type="text" name="LAST_NAME" required>
    <input type="text" name="PHONE" required>
    <input type="submit" name="SAVE" value="Send">
</form>
```

The user does not see the hidden field, but its value is sent along with the rest of the form data.

## 2. Retrieve Sales Intelligence Data

After the page loads, access the `b24Tracker` object and retrieve the current visitor trace. Write the value into the `TRACE` hidden field. This is client-side code—it runs in the browser on the page containing the form:

```html
<!-- The Bitrix24 end-to-end analytics script must be installed on the page -->
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
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    // name, lastName, phone come from the form data (req.body)
    const leadResponse = await $b24.actions.v2.call.make({
        method: 'crm.item.add',
        params: {
            entityTypeId: 1,
            fields: {
                title: `Feedback page: ${name} ${lastName}`,
                name: name,
                lastName: lastName,
                fm: [
                    { typeId: 'PHONE', valueType: 'WORK', value: phone },
                ],
            },
        },
        requestId: 'lead-add',
    })

    if (!leadResponse.isSuccess) {
        throw new Error(leadResponse.getErrorMessages().join('; '))
    }

    const leadId = leadResponse.getData().result.item.id
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import Client, BitrixWebhook

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="1/xxxxxxxxxxxxxxxx",
    ))

    #  name, last_name, phone come from the form data
    bitrix_response = client.crm.item.add(
        fields={
            "title": f"Feedback page: {name} {last_name}",
            "name": name,
            "lastName": last_name,
            "fm": [
                {"typeId": "PHONE", "valueType": "WORK", "value": phone},
            ],
        },
        entity_type_id=1,
    ).response
    lead_id = bitrix_response.result["item"]["id"]
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

    // $name, $lastName, $phone come from the form data ($_POST)
    $leadId = $b24->getCRMScope()->item()->add(1, [
        'title' => 'Feedback page: ' . $name . ' ' . $lastName,
        'name' => $name,
        'lastName' => $lastName,
        'fm' => [
            ['typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => $phone],
        ],
    ])->item()->id;
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
    if (trace) {
        const traceResponse = await $b24.actions.v2.call.make({
            method: 'crm.tracking.trace.add',
            params: {
                TRACE: trace,
                ENTITIES: [
                    { TYPE: 'LEAD', ID: leadId },
                ],
            },
            requestId: 'trace-add',
        })

        if (!traceResponse.isSuccess) {
            throw new Error(traceResponse.getErrorMessages().join('; '))
        }
    }
    ```

- Python

    ```python
    if trace:
        client.crm.tracking.trace.add(
            trace=trace,
            entities=[
                {"TYPE": "LEAD", "ID": lead_id},
            ],
        ).response
    ```


- PHP

    ```php
    if (!empty($trace)) {
        // crm.tracking.* is not among the typed services — calling directly via the core
        $b24->core->call('crm.tracking.trace.add', [
            'TRACE' => $trace,
            'ENTITIES' => [
                ['TYPE' => 'LEAD', 'ID' => $leadId],
            ],
        ]);
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

In the examples below, the backend serves an HTML page with a form and processes its submission. Replace the URL variable with your Bitrix24 webhook address.

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const WEBHOOK = 'https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    const formPage = (message = '') => `<!DOCTYPE html>
    <html lang="en">
        <head>
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        </head>
        <body class="container">
            <h1>Feedback</h1>
            <div class="col-12"><p>${message}</p></div>
            <form method="post" action="/">
                <input type="hidden" id="FORM_TRACE" name="TRACE">
                <div class="row"><div class="col-4 mt-3"><label>Name*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="NAME" required></div></div>
                <div class="row"><div class="col-4 mt-3"><label>Last name*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="LAST_NAME" required></div></div>
                <div class="row"><div class="col-4 mt-3"><label>Phone*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="PHONE" required></div></div>
                <div class="row"><div class="col-sm-10">
                    <input type="submit" name="SAVE" class="btn btn-primary" value="Send"></div></div>
            </form>
            <!-- The Bitrix24 end-to-end analytics script must be installed on the page -->
            <script>
                window.onload = function() {
                    var traceInput = document.getElementById('FORM_TRACE');
                    if (traceInput && typeof b24Tracker !== 'undefined'
                        && b24Tracker.guest && typeof b24Tracker.guest.getTrace === 'function') {
                        traceInput.value = b24Tracker.guest.getTrace();
                    }
                }
            </script>
        </body>
    </html>`

    app.get('/', (req, res) => res.send(formPage()))

    app.post('/', async (req, res) => {
        const { NAME = '', LAST_NAME = '', PHONE = '', TRACE = '' } = req.body
        const $b24 = B24Hook.fromWebhookUrl(WEBHOOK)
        try {
            const leadResponse = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                    entityTypeId: 1,
                    fields: {
                        title: `Feedback page: ${NAME} ${LAST_NAME}`,
                        name: NAME,
                        lastName: LAST_NAME,
                        fm: [{ typeId: 'PHONE', valueType: 'WORK', value: PHONE }],
                    },
                },
                requestId: 'lead-add',
            })
            if (!leadResponse.isSuccess) {
                return res.send(formPage('Lead not created: ' + leadResponse.getErrorMessages().join('; ')))
            }
            const leadId = leadResponse.getData().result.item.id

            if (TRACE) {
                await $b24.actions.v2.call.make({
                    method: 'crm.tracking.trace.add',
                    params: { TRACE, ENTITIES: [{ TYPE: 'LEAD', ID: leadId }] },
                    requestId: 'trace-add',
                })
                return res.send(formPage('Lead created'))
            }
            res.send(formPage('Lead created without trace'))
        } catch (error) {
            res.send(formPage('Error: ' + error.message))
        } finally {
            $b24.destroy()
        }
    })

    app.listen(3000, () => console.log('http://localhost:3000'))
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from flask import Flask, request
    from b24pysdk import Client, BitrixWebhook

    WEBHOOK_DOMAIN = "your-domain.bitrix24.com"
    WEBHOOK_TOKEN = "1/xxxxxxxxxxxxxxxx"

    app = Flask(__name__)

    def form_page(message: str = "") -> str:
        return f"""<!DOCTYPE html>
    <html lang="en">
        <head>
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        </head>
        <body class="container">
            <h1>Feedback</h1>
            <div class="col-12"><p>{message}</p></div>
            <form method="post" action="/">
                <input type="hidden" id="FORM_TRACE" name="TRACE">
                <div class="row"><div class="col-4 mt-3"><label>Name*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="NAME" required></div></div>
                <div class="row"><div class="col-4 mt-3"><label>Last name*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="LAST_NAME" required></div></div>
                <div class="row"><div class="col-4 mt-3"><label>Phone*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="PHONE" required></div></div>
                <div class="row"><div class="col-sm-10">
                    <input type="submit" name="SAVE" class="btn btn-primary" value="Send"></div></div>
            </form>
            <!-- The Bitrix24 end-to-end analytics script must be installed on the page -->
            <script>
                window.onload = function() {{
                    var traceInput = document.getElementById('FORM_TRACE');
                    if (traceInput && typeof b24Tracker !== 'undefined'
                        && b24Tracker.guest && typeof b24Tracker.guest.getTrace === 'function') {{
                        traceInput.value = b24Tracker.guest.getTrace();
                    }}
                }}
            </script>
        </body>
    </html>"""

    @app.get("/")
    def index():
        return form_page()

    @app.post("/")
    def submit():
        client = Client(BitrixWebhook(domain=WEBHOOK_DOMAIN, webhook_token=WEBHOOK_TOKEN))
        name = request.form.get("NAME", "")
        last_name = request.form.get("LAST_NAME", "")
        phone = request.form.get("PHONE", "")
        trace = request.form.get("TRACE", "")
        try:
            bitrix_response = client.crm.item.add(
                fields={
                    "title": f"Feedback page: {name} {last_name}",
                    "name": name,
                    "lastName": last_name,
                    "fm": [{"typeId": "PHONE", "valueType": "WORK", "value": phone}],
                },
                entity_type_id=1,
            ).response
            lead_id = bitrix_response.result["item"]["id"]
            if trace:
                client.crm.tracking.trace.add(
                    trace=trace,
                    entities=[{"TYPE": "LEAD", "ID": lead_id}],
                ).response
                return form_page("Lead created")
            return form_page("Lead created without trace")
        except Exception as error:
            return form_page(f"Lead not created: {error}")
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

    $message = '';

    if (!empty($_POST['SAVE'])) {
        $log = new Logger('b24');
        $log->pushHandler(new StreamHandler('php://stdout'));
        $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
            ->initFromWebhook('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/');

        $name = htmlspecialchars($_POST['NAME'] ?? '');
        $lastName = htmlspecialchars($_POST['LAST_NAME'] ?? '');
        $phone = htmlspecialchars($_POST['PHONE'] ?? '');
        $trace = $_POST['TRACE'] ?? '';

        try {
            $leadId = $b24->getCRMScope()->item()->add(1, [
                'title' => 'Feedback page: ' . $name . ' ' . $lastName,
                'name' => $name,
                'lastName' => $lastName,
                'fm' => [
                    ['typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => $phone],
                ],
            ])->item()->id;

            if (!empty($trace)) {
                $b24->core->call('crm.tracking.trace.add', [
                    'TRACE' => $trace,
                    'ENTITIES' => [
                        ['TYPE' => 'LEAD', 'ID' => $leadId],
                    ],
                ]);
                $message = 'Lead created';
            } else {
                $message = 'Lead created without trace';
            }
        } catch (\Throwable $e) {
            $message = 'Lead not created: ' . $e->getMessage();
        }
    }
    ?>
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        </head>
        <body class="container">
            <h1>Feedback</h1>
            <div class="col-12"><p><?=$message?></p></div>
            <form method="post" action="">
                <input type="hidden" id="FORM_TRACE" name="TRACE">
                <div class="row"><div class="col-4 mt-3"><label>Name*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="NAME" required></div></div>
                <div class="row"><div class="col-4 mt-3"><label>Last name*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="LAST_NAME" required></div></div>
                <div class="row"><div class="col-4 mt-3"><label>Phone*</label></div>
                    <div class="col-6 mt-3"><input type="text" name="PHONE" required></div></div>
                <div class="row"><div class="col-sm-10">
                    <input type="submit" name="SAVE" class="btn btn-primary" value="Send"></div></div>
            </form>
            <!-- The Bitrix24 end-to-end analytics script must be installed on the page -->
            <script>
                window.onload = function() {
                    var traceInput = document.getElementById('FORM_TRACE');
                    if (traceInput && typeof b24Tracker !== 'undefined'
                        && b24Tracker.guest && typeof b24Tracker.guest.getTrace === 'function') {
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
