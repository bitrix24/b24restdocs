# How to Use Sales Intelligence When Creating a Deal and Contact

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This example demonstrates the use of Sales Intelligence when creating a deal and a contact. First, create a PHP page with a feedback web form: Full Name, Phone. Place the example code on the page.

What happens during code execution?

1. The standard JS code from Bitrix24's Sales Intelligence is connected.
2. After filling out the form, in addition to the form fields, a code for Sales Intelligence `b24Tracker.guest.getTrace()` is passed in a hidden field.
3. A deal and a related contact are then created.
4. Finally, an analytics "trace" is registered for these objects, passing their types and identifiers in the following format:

```bash
/rest/crm.tracking.trace.add?ENTITIES[0][TYPE]=CONTACT&ENTITIES[0][ID]=3215&ENTITIES[1][TYPE]=DEAL&ENTITIES[1][ID]=1&TRACE=…
```

The Sales Intelligence script is installed on your website before the `</body>` closing tag on all website pages, including the page with the form.

{% note info "" %}

The form on the website is public; therefore, REST calls are performed on the server side rather than in the browser: a webhook with CRM permissions must not be exposed in client-side code. The browser collects the form data and the trace and sends them to the backend via a standard POST request. The backend calls Bitrix24 methods via the SDK: PHP — [B24PhpSDK](https://github.com/bitrix24/b24phpsdk), Python — [b24pysdk](https://github.com/bitrix24/b24pysdk), JS — [b24jssdk](https://github.com/bitrix24/b24jssdk) on the server (Node.js).

{% endnote %}

## Sequence of Calls

The contact, deal, and trace are created sequentially: the contact identifier is passed to the deal, and the identifiers of both objects are passed to the trace.

1. [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) — create a contact
2. [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) — create a deal with `CONTACT_ID`
3. [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) — link the contact and deal with a single trace

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    // name, lastName, phone, trace come from the form data
    const contactResponse = await $b24.actions.v2.call.make({
        method: 'crm.contact.add',
        params: { fields: { NAME: name, LAST_NAME: lastName, PHONE: [{ value: phone }] } },
        requestId: 'contact-add',
    })
    if (!contactResponse.isSuccess) throw new Error(contactResponse.getErrorMessages().join('; '))
    const contactId = contactResponse.getData().result

    const dealResponse = await $b24.actions.v2.call.make({
        method: 'crm.deal.add',
        params: { fields: { TITLE: `Feedback page: ${name} ${lastName}`, CONTACT_ID: contactId } },
        requestId: 'deal-add',
    })
    if (!dealResponse.isSuccess) throw new Error(dealResponse.getErrorMessages().join('; '))
    const dealId = dealResponse.getData().result

    if (trace) {
        await $b24.actions.v2.call.make({
            method: 'crm.tracking.trace.add',
            params: {
                ENTITIES: [
                    { TYPE: 'CONTACT', ID: contactId },
                    { TYPE: 'DEAL', ID: dealId },
                ],
                TRACE: trace,
            },
            requestId: 'trace-add',
        })
    }

    $b24.destroy()
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

    // $name, $lastName, $phone, $trace come from the form data
    $contactId = $b24->getCRMScope()->contact()->add([
        'NAME' => $name,
        'LAST_NAME' => $lastName,
        'PHONE' => [['value' => $phone]],
    ])->getId();

    $dealId = $b24->getCRMScope()->deal()->add([
        'TITLE' => 'Feedback page: ' . $name . ' ' . $lastName,
        'CONTACT_ID' => $contactId,
    ])->getId();

    if (!empty($trace)) {
        // crm.tracking.* is not among the typed services — calling directly via the core
        $b24->core->call('crm.tracking.trace.add', [
            'ENTITIES' => [
                ['TYPE' => 'CONTACT', 'ID' => $contactId],
                ['TYPE' => 'DEAL', 'ID' => $dealId],
            ],
            'TRACE' => $trace,
        ]);
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import Client, BitrixWebhook

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="1/xxxxxxxxxxxxxxxx",
    ))

    # name, last_name, phone, trace come from the form data
    contact_id = client.crm.contact.add(
        fields={"NAME": name, "LAST_NAME": last_name, "PHONE": [{"value": phone}]},
    ).response.result

    deal_id = client.crm.deal.add(
        fields={"TITLE": f"Feedback page: {name} {last_name}", "CONTACT_ID": contact_id},
    ).response.result

    if trace:
        client.crm.tracking.trace.add(
            trace=trace,
            entities=[
                {"TYPE": "CONTACT", "ID": contact_id},
                {"TYPE": "DEAL", "ID": deal_id},
            ],
        ).response
    ```

{% endlist %}

The [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) and [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) methods return the identifier of the created object in the `result` field. The [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method returns the trace identifier.

## Full Code Example

The backend serves an HTML page with a feedback form and processes its submission. The Sales Intelligence script is connected to the page, which populates the hidden field `TRACE`.

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
            <script>
                window.onload = function() {
                    var traceDom = document.getElementById('FORM_TRACE');
                    if (traceDom && typeof b24Tracker !== 'undefined' && b24Tracker.guest) {
                        traceDom.value = b24Tracker.guest.getTrace();
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
            const contactResponse = await $b24.actions.v2.call.make({
                method: 'crm.contact.add',
                params: { fields: { NAME, LAST_NAME, PHONE: [{ value: PHONE }] } },
                requestId: 'contact-add',
            })
            if (!contactResponse.isSuccess) {
                return res.send(formPage('Feedback has not been saved: ' + contactResponse.getErrorMessages().join('; ')))
            }
            const contactId = contactResponse.getData().result

            const dealResponse = await $b24.actions.v2.call.make({
                method: 'crm.deal.add',
                params: { fields: { TITLE: `Feedback page: ${NAME} ${LAST_NAME}`, CONTACT_ID: contactId } },
                requestId: 'deal-add',
            })
            if (!dealResponse.isSuccess) {
                return res.send(formPage('Feedback has not been saved: ' + dealResponse.getErrorMessages().join('; ')))
            }
            const dealId = dealResponse.getData().result

            if (TRACE) {
                await $b24.actions.v2.call.make({
                    method: 'crm.tracking.trace.add',
                    params: {
                        ENTITIES: [
                            { TYPE: 'CONTACT', ID: contactId },
                            { TYPE: 'DEAL', ID: dealId },
                        ],
                        TRACE,
                    },
                    requestId: 'trace-add',
                })
            }
            res.send(formPage('Feedback saved'))
        } catch (error) {
            res.send(formPage('Feedback has not been saved: ' + error.message))
        } finally {
            $b24.destroy()
        }
    })

    app.listen(3000, () => console.log('http://localhost:3000'))
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

        $name = $_POST['NAME'] ?? '';
        $lastName = $_POST['LAST_NAME'] ?? '';
        $phone = $_POST['PHONE'] ?? '';
        $trace = $_POST['TRACE'] ?? '';

        try {
            $contactId = $b24->getCRMScope()->contact()->add([
                'NAME' => $name,
                'LAST_NAME' => $lastName,
                'PHONE' => [['value' => $phone]],
            ])->getId();

            $dealId = $b24->getCRMScope()->deal()->add([
                'TITLE' => 'Feedback page: ' . $name . ' ' . $lastName,
                'CONTACT_ID' => $contactId,
            ])->getId();

            if (!empty($trace)) {
                $b24->core->call('crm.tracking.trace.add', [
                    'ENTITIES' => [
                        ['TYPE' => 'CONTACT', 'ID' => $contactId],
                        ['TYPE' => 'DEAL', 'ID' => $dealId],
                    ],
                    'TRACE' => $trace,
                ]);
            }
            $message = 'Feedback saved';
        } catch (\Throwable $e) {
            $message = 'Feedback has not been saved: ' . $e->getMessage();
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
            <script>
                window.onload = function() {
                    var traceDom = document.getElementById('FORM_TRACE');
                    if (traceDom && typeof b24Tracker !== 'undefined' && b24Tracker.guest) {
                        traceDom.value = b24Tracker.guest.getTrace();
                    }
                }
            </script>
        </body>
    </html>
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
            <script>
                window.onload = function() {{
                    var traceDom = document.getElementById('FORM_TRACE');
                    if (traceDom && typeof b24Tracker !== 'undefined' && b24Tracker.guest) {{
                        traceDom.value = b24Tracker.guest.getTrace();
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
            contact_id = client.crm.contact.add(
                fields={"NAME": name, "LAST_NAME": last_name, "PHONE": [{"value": phone}]},
            ).response.result
            deal_id = client.crm.deal.add(
                fields={"TITLE": f"Feedback page: {name} {last_name}", "CONTACT_ID": contact_id},
            ).response.result
            if trace:
                client.crm.tracking.trace.add(
                    trace=trace,
                    entities=[
                        {"TYPE": "CONTACT", "ID": contact_id},
                        {"TYPE": "DEAL", "ID": deal_id},
                    ],
                ).response
            return form_page("Feedback saved")
        except Exception as error:
            return form_page(f"Feedback has not been saved: {error}")
    ```

{% endlist %}

## Continue Learning

- [{#T}](./info-to-analitics.md)
- [{#T}](./use-analitics-for-add-lead.md)
- [{#T}](../../../api-reference/crm/tracking/crm-tracking-trace-add.md)
