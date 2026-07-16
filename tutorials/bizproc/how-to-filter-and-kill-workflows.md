# How to Mass Terminate Workflows with Date Filtering

> Scope: [`bizproc`](../../api-reference/scopes/permissions.md)
> 
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

During the operation of Bitrix24, you may accumulate stuck workflows or processes that remain in the "In Progress" status for too long and become irrelevant.

To mass terminate old workflows, we will sequentially execute two methods:
1. [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) — retrieve a filtered list of processes
2. [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) — terminate workflows with data deletion. If you need to preserve the fact that the workflow was initiated, use the [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md) method. Both methods are called in the same way.

## 1. Retrieve the List of Processes {#workflow_id}

We will use the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method with a filter:

- `<STARTED` — specify the launch date with the `<` prefix; only workflows launched before this date will be selected.

{% list tabs %}

- JS
  
    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.instances',
        params: {
            filter: { '<STARTED': '2025-01-01T00:00:00Z' },
        },
        requestId: 'workflow-instances',
    })

    const instances = response.getData().result
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

    $instances = $b24->getBizProcScope()->workflow()->instances(
        filter: ['<STARTED' => '2025-01-01T00:00:00Z']
    )->getInstances();
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    response = client.bizproc.workflow.instances(
        filter={
            "<STARTED": "2025-01-01T00:00:00Z",
        }
    ).response
    ```

{% endlist %}

As a result, we will obtain the `ID` of all active workflows that were initiated before the specified date.

```json
{
    "result": [
        {
            "ID": "660e559f34af10.95144732",
            "MODIFIED": "2024-12-04T10:04:24+03:00",
            "OWNED_UNTIL": null
        },
        {
            "ID": "6639c7b59e9eb5.40607056",
            "MODIFIED": "2024-12-04T09:52:40+03:00",
            "OWNED_UNTIL": null
        }
    ],
    "total": 2,
}
```

## 2. Terminate Workflows 

Use the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method with the following parameter:
- `ID` — the workflow identifier; pass the `ID` obtained in [step 1](#workflow_id).

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.kill',
        params: { ID: '660e559f34af10.95144732' },
        requestId: 'workflow-kill',
    })

    const isKilled = response.getData().result
    ```

- PHP

    ```php
    $isKilled = $b24->getBizProcScope()->workflow()
        ->kill('660e559f34af10.95144732')
        ->isSuccess();
    ```

- Python

    ```python
    # Process ID is a string, so we call the method directly via token.call_method
    # (typed client.bizproc.workflow.kill expects an int)
    response = token.call_method(
        "bizproc.workflow.kill",
        {"ID": "660e559f34af10.95144732"},
    )
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the process was successfully deleted. If you encounter an `error`, refer to the documentation for possible errors in the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method.

```json
{
    "result": true,
}
```

## Code Example

In the example, all found workflows are deleted within a loop. When deleting large volumes of data, request execution limits may apply. To optimize the code for your workload, use the recommendations in the [Performance](../../settings/performance/index.md) section.

{% list tabs %}

- JS
  
    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    // Enter the date in dd.mm.yyyy format as an argument: node kill.mjs 01.01.2025
    const [day, month, year] = (process.argv[2] || '').split('.')
    const isoDate = `${year}-${month}-${day}T00:00:00Z`

    // callList iterates through all selection pages itself; getData() returns an array of elements
    const listResponse = await $b24.actions.v2.callList.make({
        method: 'bizproc.workflow.instances',
        params: { filter: { '<STARTED': isoDate }, select: ['ID'] },
        requestId: 'workflow-instances',
    })

    const instances = listResponse.getData()

    for (const instance of instances) {
        const response = await $b24.actions.v2.call.make({
            method: 'bizproc.workflow.kill',
            params: { ID: instance.ID },
            requestId: `kill-${instance.ID}`,
        })
        console.log(response.isSuccess
            ? `Process ${instance.ID} successfully deleted.`
            : `Error deleting process ${instance.ID}: ${response.getErrorMessages().join('; ')}`)
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

    $userDateInput = readline('Enter the date in dd.mm.yyyy format: ');
    [$day, $month, $year] = explode('.', $userDateInput);
    $isoDate = "{$year}-{$month}-{$day}T00:00:00Z";

    // The instances() method returns a single page. For pagination
    // we call the method directly via the core and read the offset of the next page.
    $start = 0;
    do {
        $response = $b24->core->call('bizproc.workflow.instances', [
            'filter' => ['<STARTED' => $isoDate],
            'start' => $start,
        ]);

        foreach ($response->getResponseData()->getResult() as $instance) {
            $isKilled = $b24->getBizProcScope()->workflow()->kill($instance['ID'])->isSuccess();
            echo $isKilled
                ? "Process {$instance['ID']} successfully deleted.\n"
                : "Error deleting process {$instance['ID']}\n";
        }

        $start = $response->getResponseData()->getPagination()->getNextItem();
    } while ($start !== null);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    user_date_input = input("Enter the date in dd.mm.yyyy format: ")
    day, month, year = user_date_input.split(".")
    iso_date = f"{year}-{month}-{day}T00:00:00Z"

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    start = None
    while True:
        kwargs = {"filter": {"<STARTED": iso_date}}
        if start is not None:
            kwargs["start"] = start

        response = client.bizproc.workflow.instances(**kwargs).response
        instances = response.result or []

        for instance in instances:
            instance_id = instance["ID"]
            try:
                token.call_method("bizproc.workflow.kill", {"ID": instance_id})
            except BitrixAPIError as error:
                print(f"Error deleting process {instance_id}: {error}")
            else:
                print(f"Process {instance_id} successfully deleted.")

        if response.next is None:
            break
        start = response.next
    ```

{% endlist %}
