# How to pass data to Sales Intelligence

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: a user with permission to create or edit a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Sales Intelligence data helps link a lead, deal, contact, company, or estimate to the source of the request and the customer journey. In CRM, you can pass either just the source via UTM fields or a full trace with visit data.

A trace is a set of data about the customer's path before the request: the referral source, pages visited, and other visit parameters. Using the trace, CRM understands where the customer came from and what actions they took before the object was created.

To pass data to Sales Intelligence, choose a method:

#|
|| **If needed** | **What to pass** | **Which methods to rely on** ||
|| Pass only the advertising source when creating an object | UTM fields: `UTM_SOURCE` and others | [CRM object creation methods](../../../api-reference/crm/index.md) ||
|| Pass the full customer journey when creating an object | `TRACE`, if the creation method supports this field | [CRM object creation methods](../../../api-reference/crm/index.md) ||
|| Link one trace to several or to already created objects | `TRACE` and an array of `ENTITIES` objects | [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) ||
|#

The scenario consists of four steps.

1. Choose whether UTM fields are sufficient or a full trace is required
2. Retrieve the `TRACE` string using `b24Tracker.guest.getTrace()` if you need to pass the full customer journey
3. Pass `TRACE` when creating an object if the creation method supports this field, or link the trace to existing objects using [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md)
4. Retain the trace identifier from `result` if you need to delete it later using [crm.tracking.trace.delete](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md)

## Before You Start

- an incoming webhook or a local application with scope [`crm`](../../../api-reference/scopes/permissions.md)
- user permissions to create or edit the CRM object that should receive Sales Intelligence data
- the Bitrix24 Sales Intelligence script is installed on the website pages where the customer journey is collected
- REST calls are executed on the server side if you use a webhook: the webhook path must not be exposed in the browser or a public repository
- identifiers of already created CRM objects if the trace needs to be linked using [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md)

## 1. Pass the UTM source

If the advertising source is sufficient for the report, pass `UTM_SOURCE` when creating a CRM object. The value must match the configured source in Sales Intelligence.

Main CRM objects have UTM fields. Check the list of fields in the description of the method you use to create the object:

- [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — lead
- [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) — deal
- [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) — contact
- [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) — company
- [crm.quote.add](../../../api-reference/crm/quote/crm-quote-add.md) — estimate

This method is suitable when you only need to pass the acquisition channel: advertising system, campaign, ad, or keyword.

The universal method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) accepts UTM fields in `camelCase`, for example `utmSource`, and saves them in an object. However, it does not form the customer journey in Sales Intelligence: the trace is not created, and the `TRACE` field is not available in the method.

To ensure the data reaches Sales Intelligence, create the object using specific CRM methods or separately link a trace via [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md).

## 2. Pass a full trace when creating an object

A full trace contains data about the customer journey: source, website pages, and other visit parameters. The value for `TRACE` can be retrieved on a website via the Bitrix24 Sales Intelligence JS code:

```js
b24Tracker.guest.getTrace()
```

The Sales Intelligence script must be installed on the website pages where the customer journey is collected. Typically, the `TRACE` value is stored in a hidden form field and sent along with the customer data.

If the object creation method supports the `TRACE` field, pass the retrieved string into it. This option is suitable when an object is created immediately after a form is submitted. For example, a request from a website creates a lead or contact, and Sales Intelligence data is passed along with the main object fields.

For detailed parameters and examples, see the description of the method for creating the required object. Practical scenarios show how to pass `TRACE` during creation:

- [lead](./use-analitics-for-add-lead.md)
- [deal and contact](./use-analitics-for-add-contact.md)

## 3. Link objects with a single trace

If a scenario creates multiple related objects, first create or save the client records, then link them to a single trace using the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method.

For example, a website form might create a contact and a deal. After creating the objects, pass the following to [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md):

- `TRACE` — a string containing Sales Intelligence data
- `ENTITIES` — a list of objects to be linked to the trace

Perform REST calls on the server side to avoid exposing the webhook in the browser. Assemble the `TRACE` string on the website via `b24Tracker.guest.getTrace()` and pass it to the server along with the form data.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    // contactId and dealId were obtained during object creation, trace — from b24Tracker.guest.getTrace()
    const response = await $b24.actions.v2.call.make({
        method: 'crm.tracking.trace.add',
        params: {
            TRACE: trace,
            ENTITIES: [
                { TYPE: 'CONTACT', ID: contactId },
                { TYPE: 'DEAL', ID: dealId },
            ],
        },
        requestId: 'trace-add',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const traceId = response.getData().result
    console.log('Trace ID:', traceId)

    $b24.destroy()
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import Client, BitrixWebhook
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="1/xxxxxxxxxxxxxxxx",
    ))

    try:
        bitrix_response = client.crm.tracking.trace.add(
            trace=trace,
            entities=[
                {"TYPE": "CONTACT", "ID": contact_id},
                {"TYPE": "DEAL", "ID": deal_id},
            ],
        ).response
        trace_id = bitrix_response.result
        print("Trace ID:", trace_id)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

    // The crm.tracking.* method is not among the typed services of the SDK,
    // therefore we call it directly via the core: $b24->core->call(...)
    $response = $b24->core->call('crm.tracking.trace.add', [
        'TRACE' => $trace,
        'ENTITIES' => [
            ['TYPE' => 'CONTACT', 'ID' => $contactId],
            ['TYPE' => 'DEAL', 'ID' => $dealId],
        ],
    ]);

    // The core wraps the scalar result (trace ID) in an array
    $traceId = $response->getResponseData()->getResult()[0];
    echo 'Trace ID: ' . $traceId;
    ```
{% endlist %}

This method also works for objects created via the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method: a trace can be linked to them after creation.

The method returns the identifier of the created trace. You can retain this identifier on the integration side if the scenario requires deleting the trace or clearing the link later.

```json
{
    "result": 341
}
```

Retain the `result` value. In this example, it is `341`: the identifier that must be passed in the `id` parameter of [crm.tracking.trace.delete](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md) if you need to delete the trace.

## Deleting a trace

Delete a trace if it was erroneously linked to an object or if you need to clear test data.

To delete a trace, use the [crm.tracking.trace.delete](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md) method. Specify the trace identifier `id` returned by the [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'crm.tracking.trace.delete',
        params: { id: traceId },
        requestId: 'trace-delete',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }
    ```

- Python

    ```python
    bitrix_response = client.crm.tracking.trace.delete(traceId).response
    result = bitrix_response.result
    print(result)
    ```


- PHP

    ```php
    $response = $b24->core->call('crm.tracking.trace.delete', [
        'id' => $traceId,
    ]);

    $isDeleted = $response->getResponseData()->getResult()[0];
    ```
{% endlist %}

If deletion is successful, the method returns `null` in the `result` field.

```json
{
    "result": null
}
```

## Check the Result

Verification depends on the selected data transfer method.

#|
|| **Method** | **How to Check** ||
|| UTM fields when creating an object | Open the created CRM object and check that the UTM fields are filled with values from the form or integration ||
|| `TRACE` when creating an object | Open the created CRM object and check the *Sales Intelligence* field ||
|| [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) | Make sure the method returned a numeric `result`. Then open the objects from `ENTITIES` and check the *Sales Intelligence* field ||
|| [crm.tracking.trace.delete](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md) | Make sure the method returned `result: null` and the *Sales Intelligence* field value was cleared in the linked object ||
|#

If the object was created but the trace was not linked, the scenario completed partially. Check the object before running the scenario again to avoid creating a duplicate.

## Error Handling

The [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md) method returns code `ERROR_CORE` in the `error` field if validation of the `TRACE` parameter, `ENTITIES` parameter, or object permissions fails. The specific reason is passed in the `error_description` field.

| `error_description` | Reason | What to Check | Which Step to Repeat |
|---|---|---|---|
| ``Parameter `TRACE` required.`` | The trace was not passed | Loading of the Sales Intelligence script, the `b24Tracker.guest.getTrace()` call, and passing the value to the server | From step 2 |
| ``Can not parse JSON in parameter `TRACE`.`` | `TRACE` is not a valid JSON string | Make sure `TRACE` contains the result of `b24Tracker.guest.getTrace()` without format changes | From step 2 |
| ``Wrong TYPE in parameter `ENTITIES`. Allowed types: COMPANY,CONTACT,DEAL,LEAD,QUOTE`` | An unsupported object type was passed | `TYPE` values: `COMPANY`, `CONTACT`, `DEAL`, `LEAD`, `QUOTE` are available | From step 3 |
| ``Wrong ID in parameter `ENTITIES`.`` | An empty, non-numeric, or non-positive object identifier was passed | Object identifiers from responses of CRM creation or retrieval methods | From step 3 |
| ``You have no access to entity `CONTACT` with ID `123`.`` | No permission to edit the object from `ENTITIES` | Permissions of the user on whose behalf the REST call is executed | After configuring permissions, repeat step 3 |

In the last message, `CONTACT` and `123` are examples. The method inserts the actual object type and identifier.

The [crm.tracking.trace.delete](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md) method returns the `ERROR_CORE` error with description ``Parameter `id` required.`` if the trace identifier is missing or an empty value is passed. Check that `id` is taken from the `result` field of [crm.tracking.trace.add](../../../api-reference/crm/tracking/crm-tracking-trace-add.md), and repeat the deletion.

System REST errors for authorization, permissions, and limits may have other codes. If a REST call returns an error, do not repeat the whole scenario immediately: first check which objects have already been created and whether a trace was created.

## Continue Learning

- [{#T}](../../../api-reference/crm/tracking/crm-tracking-trace-add.md)
- [{#T}](../../../api-reference/crm/tracking/crm-tracking-trace-delete.md)
- [{#T}](./use-analitics-for-add-lead.md)
- [{#T}](./use-analitics-for-add-contact.md)
