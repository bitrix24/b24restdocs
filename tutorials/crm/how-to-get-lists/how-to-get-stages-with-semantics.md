# How to Retrieve a List of Stages with Semantics for CRM Entities

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user with permission to read at least one CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The semantics of a stage reflects the current state of a CRM object: in progress, successfully completed, or unsuccessful. The system uses the semantic value in automation and reporting.

The stages of any CRM object are returned by a single method — [crm.status.list](../../../api-reference/crm/status/crm-status-list.md). It returns the stages of one directory, which is set by the `ENTITY_ID` code in the filter. As a result, we get a list of stages with semantics for the selected object.

The scenario consists of two steps.

1. Determine the `ENTITY_ID` directory code for the required CRM object.
2. Retrieve the stages of that directory using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method and read the semantics of each stage.

## 1. Determine the Directory Code {#entity-id}

The directory code depends on the CRM object and on the pipeline.

#|
|| **CRM object** | **`ENTITY_ID` code** | **Where to get the numeric part** ||
|| [Leads](../../../api-reference/crm/leads/index.md) | `STATUS` | A constant code, there is no numeric part ||
|| [Deals](../../../api-reference/crm/deals/index.md) | `DEAL_STAGE` for the main pipeline, `DEAL_STAGE_{categoryId}` for the others | `categoryId` — the deal pipeline identifier from the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with `entityTypeId`: `2` ||
|| [Quotes](../../../api-reference/crm/quote/index.md) | `QUOTE_STATUS` | A constant code, there is no numeric part ||
|| [Invoices](../../../api-reference/crm/universal/invoice.md) | `SMART_INVOICE_STAGE_{categoryId}` | `categoryId` — the invoice pipeline identifier from the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with `entityTypeId`: `31` ||
|| [Documents](https://helpdesk.bitrix24.com/open/19441484/) | `SMART_DOCUMENT_STAGE_{categoryId}` | `categoryId` — the document pipeline identifier from the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with `entityTypeId`: `36` ||
|| [Smart Processes](../../../api-reference/crm/universal/index.md) | `DYNAMIC_{entityTypeId}_STAGE_{categoryId}` | `entityTypeId` — the smart process type identifier from the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method, `categoryId` — the pipeline identifier from the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with that `entityTypeId` ||
|#

The smart process code uses `entityTypeId`, not the `id` from the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) response. These are different numbers: for a smart process with `id`: `7`, the `entityTypeId` value is `177`, so the directory code of its main pipeline is `DYNAMIC_177_STAGE_7`, not `DYNAMIC_7_STAGE_7`.

For leads, the code is constant, so we use `STATUS` in the examples below.

## 2. Retrieve the Stages with Semantics

Call the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the following parameters:

- `filter` — specify the `ENTITY_ID` field with the value `STATUS` to get the lead stages
- `order` — sort by the `SORT` field in ascending order so that the stages follow the same order as in the interface

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.list',
        params: {
            order: { SORT: 'ASC' }, // sort by ascending value in the SORT field
            filter: { ENTITY_ID: 'STATUS' }, // get stages for leads
        },
        requestId: 'status-list'
    });

    if (result.isSuccess) {
        console.dir(result.getData().result);
    } else {
        console.error(result.getErrorMessages().join('; '));
    }
    ```

-  PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $statuses = $sb->getCRMScope()->status()->list(
        ['SORT' => 'ASC'], // sort by ascending value in the SORT field
        ['ENTITY_ID' => 'STATUS'] // get stages for leads
    )->getStatuses();
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.status.list(
        order={"SORT": "ASC"},  # sort by ascending value in the SORT field
        filter={"ENTITY_ID": "STATUS"},  # get stages for leads
    ).response.result
    ```

{% endlist %}

As a result, we will receive an array of objects, where each object is a description of a stage. The response is shortened, showing the first and the last two stages.

```json
{
    "result": [
        {
            "ID": "1",
            "ENTITY_ID": "STATUS",
            "STATUS_ID": "NEW",
            "NAME": "Not processed",
            "NAME_INIT": "Not processed",
            "SORT": "10",
            "SYSTEM": "Y",
            "CATEGORY_ID": null,
            "COLOR": "#00FFFF",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#00FFFF"
            }
        },
        {
            "ID": "15",
            "ENTITY_ID": "STATUS",
            "STATUS_ID": "CONVERTED",
            "NAME": "Converted",
            "NAME_INIT": "Converted",
            "SORT": "50",
            "SYSTEM": "Y",
            "CATEGORY_ID": null,
            "COLOR": "#37B44A",
            "SEMANTICS": "S",
            "EXTRA": {
                "SEMANTICS": "success",
                "COLOR": "#37B44A"
            }
        },
        {
            "ID": "17",
            "ENTITY_ID": "STATUS",
            "STATUS_ID": "JUNK",
            "NAME": "Low-quality lead",
            "NAME_INIT": "Low-quality lead",
            "SORT": "60",
            "SYSTEM": "Y",
            "CATEGORY_ID": null,
            "COLOR": "#F54819",
            "SEMANTICS": "F",
            "EXTRA": {
                "SEMANTICS": "failure",
                "COLOR": "#F54819"
            }
        }
    ],
    "total": 6
}
```

## Read the Semantics from the Response {#semantics}

The method returns the semantics in two different fields, and which one is filled in depends on the CRM object.

- `EXTRA.SEMANTICS` — the text semantics: `process`, `success`, or `failure`. The method adds the `EXTRA` object only for leads, deals, and quotes, that is, for the codes `STATUS`, `DEAL_STAGE`, `DEAL_STAGE_{categoryId}`, and `QUOTE_STATUS`

- `SEMANTICS` — the short semantics: `null`, `S`, or `F`. This field is filled in for all CRM objects, including invoices, documents, and smart processes

{% note warning "" %}

Code that reads only `EXTRA.SEMANTICS` gets an empty value on every stage of invoices, documents, and smart processes. Their successful and unsuccessful final stages are then wrongly grouped as "in progress".

{% endnote %}

For the code to work with any CRM object, read `EXTRA.SEMANTICS`, and when it is missing, convert the short `SEMANTICS` value into a text one using the table.

#|
|| **`SEMANTICS`** | **`EXTRA.SEMANTICS`** | **State of the CRM object** ||
|| `null` | `process` | The object is in progress ||
|| `S` | `success` | The work with the object was completed successfully ||
|| `F` | `failure` | The work with the object was completed unsuccessfully ||
|#

For comparison, here is the response for the main pipeline of a smart process with `entityTypeId`: `177`, that is, with the filter `ENTITY_ID`: `DYNAMIC_177_STAGE_7`. The objects have no `EXTRA` key, the semantics is present only in the `SEMANTICS` field.

```json
{
    "result": [
        {
            "ID": "263",
            "ENTITY_ID": "DYNAMIC_177_STAGE_7",
            "STATUS_ID": "DT177_7:NEW",
            "NAME": "Start",
            "NAME_INIT": "Start",
            "SORT": "10",
            "SYSTEM": "Y",
            "CATEGORY_ID": "7",
            "COLOR": "#22B9FF",
            "SEMANTICS": null
        },
        {
            "ID": "269",
            "ENTITY_ID": "DYNAMIC_177_STAGE_7",
            "STATUS_ID": "DT177_7:SUCCESS",
            "NAME": "Success",
            "NAME_INIT": "Success",
            "SORT": "40",
            "SYSTEM": "Y",
            "CATEGORY_ID": "7",
            "COLOR": "#00ff00",
            "SEMANTICS": "S"
        },
        {
            "ID": "271",
            "ENTITY_ID": "DYNAMIC_177_STAGE_7",
            "STATUS_ID": "DT177_7:FAIL",
            "NAME": "Failure",
            "NAME_INIT": "Failure",
            "SORT": "50",
            "SYSTEM": "Y",
            "CATEGORY_ID": "7",
            "COLOR": "#ff0000",
            "SEMANTICS": "F"
        }
    ],
    "total": 5
}
```

## Verify the Result

The scenario is complete if the response contains stages and the semantics is determined for each of them.

- The `result` array in the response is not empty, and the `total` field matches the number of stages of the selected pipeline in the Bitrix24 interface

- Exactly one stage has `SEMANTICS`: `S`. Every pipeline is built this way: it has a single successful final stage. There can be several unsuccessful final stages with the value `F`, and the remaining stages have the value `null`

- The `NAME` values match the stage names in the Kanban of the CRM object. Lead stages are available in CRM → Leads → Kanban, and smart process stages are in its Kanban on the tab of the required pipeline

If the response contains a single object whose `NAME` matches the stage name and whose `ENTITY_ID` matches the code from step 1, you can check the semantics pointwise: the final successful stage has `SEMANTICS` equal to `S`.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `400` `Access denied.` | The user does not have permission to read CRM objects. Check which user the webhook was created on behalf of ||
|| `400` `Invalid parameters.` | Incorrect values were passed in `filter` or `order`. The set of fields available for filtering and sorting is returned by the [crm.status.fields](../../../api-reference/crm/status/crm-status-fields.md) method ||
|| `400` `Filter by ENTITY_ID must be a string` | The `ENTITY_ID` field was passed in `filter` as an array. The method filters by one directory per call — pass a string, and make several calls for several objects ||
|#

The method may return an empty `result` without an error. This means that Bitrix24 has no directory with that code.

- Check the numeric part of the code. For smart processes, a common cause is `id` instead of `entityTypeId`: the code `DYNAMIC_7_STAGE_7` returns an empty list, while `DYNAMIC_177_STAGE_7` returns the stages

- Check the pipeline identifier. The pipeline may have been deleted, or the object may not have one at all — the current list is returned by the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method

The method does not modify anything, so the call can be repeated any number of times after an error.

## Key Considerations

- The method ignores the `select` parameter and always returns the full set of stage fields. You cannot narrow the selection on the API side, so pick the fields you need in your own code

- Every pipeline has its own stage directory. You cannot retrieve the stages of all pipelines of one object in a single call — iterate over the pipelines from the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method and call [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) for each code

- `STATUS_ID` is unique only within its own directory. Stages of different pipelines share both codes and names, so retain the stage together with its `ENTITY_ID`

- The `SEMANTICS` field in the filter accepts only a string. You cannot set the value `null` in the filter — select the in-progress stages in your own code

- The "Company document" object has a separate directory code — `SMART_B2E_DOC_STAGE_{categoryId}`, and its `entityTypeId` is `39`. It follows the same rules as the other codes with a numeric part

## Code Example

The code outputs tables with a list of stages for leads and for the main pipeline of a smart process. The semantics is determined universally, so the table is built the same way for an object with `EXTRA` and without it.

Replace `DYNAMIC_177_STAGE_7` with the directory code of your own object from step 1.

{% list tabs %}

-  JS

   ```javascript
   import { B24Hook } from '@bitrix24/b24jssdk'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   // Short semantics to text: there are only three of these values
   const SEMANTICS_MAP = { S: 'success', F: 'failure' };

   /**
    * Loads all stages for the specified ENTITY_ID
    * @param {string} entityId — directory code, for example, 'STATUS' or 'DYNAMIC_177_STAGE_7'
    * @returns {Promise<Array>} — array of all stages
    */
   async function loadStatuses(entityId) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.status.list',
           params: {
               filter: { ENTITY_ID: entityId },
               order: { SORT: 'ASC' }
           },
           requestId: 'status-list'
       });
       if (!result.isSuccess) {
           throw new Error(result.getErrorMessages().join('; '));
       }
       return result.getData().result;
   }

   /**
    * Returns the stage semantics for any CRM object.
    * EXTRA is present only for leads, deals, and quotes, so for the other
    * objects we convert the short SEMANTICS value into a text one
    */
   function getSemantics(item) {
       return item.EXTRA?.SEMANTICS || SEMANTICS_MAP[item.SEMANTICS] || 'process';
   }

   /**
    * Groups stages by semantics
    */
   function groupStatusesBySemantics(statuses) {
       const groups = { success: [], process: [], failure: [] };

       statuses.forEach(item => {
           const name = item.NAME || item.STATUS_ID;
           groups[getSemantics(item)].push(name);
       });

       return groups;
   }

   /**
    * Formats groups for console.table
    */
   function formatForConsoleTable(groups) {
       const { success, process, failure } = groups;
       const maxLen = Math.max(success.length, process.length, failure.length);

       const pad = (arr, len) => [...arr, ...Array(len - arr.length).fill('')];

       return Array(maxLen).fill().map((_, i) => ({
           'Success': pad(success, maxLen)[i],
           'In progress': pad(process, maxLen)[i],
           'Failure': pad(failure, maxLen)[i]
       }));
   }

   // Requesting the stages: leads keep the semantics in EXTRA, the smart process — in SEMANTICS
   Promise.all([
       loadStatuses('STATUS').then(data => ({ type: 'Leads', data })),
       loadStatuses('DYNAMIC_177_STAGE_7').then(data => ({ type: 'Smart process', data }))
   ]).then(results => {
       results.forEach(({ type, data }) => {
           console.group(type);
           const groups = groupStatusesBySemantics(data);
           console.table(formatForConsoleTable(groups));
           console.groupEnd();
       });
   }).catch(err => {
       console.error('Loading error:', err);
   });
   ```

-  PHP

   ```php
   <?php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Bitrix24\SDK\Services\ServiceBuilder;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Monolog\Logger;
   use Monolog\Handler\StreamHandler;

   $log = new Logger('b24');
   $log->pushHandler(new StreamHandler('php://stdout'));

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   // Short semantics to text: there are only three of these values
   const SEMANTICS_MAP = ['S' => 'success', 'F' => 'failure'];

   /**
    * Gets all stages for the specified ENTITY_ID
    */
   function loadStatuses(ServiceBuilder $sb, string $entityId): array {
       return $sb->getCRMScope()->status()->list(
           ['SORT' => 'ASC'],
           ['ENTITY_ID' => $entityId]
       )->getStatuses();
   }

   /**
    * Returns the stage semantics for any CRM object.
    * EXTRA is present only for leads, deals, and quotes, so for the other
    * objects we convert the short SEMANTICS value into a text one
    */
   function getSemantics($item): string {
       $extra = $item->EXTRA['SEMANTICS'] ?? '';
       if ($extra !== '') {
           return $extra;
       }
       return SEMANTICS_MAP[$item->SEMANTICS] ?? 'process';
   }

   /**
    * Groups stages by semantics
    */
   function groupStatusesBySemantics(array $statuses): array {
       $groups = ['success' => [], 'process' => [], 'failure' => []];

       foreach ($statuses as $item) {
           $name = $item->NAME ?? $item->STATUS_ID;
           $groups[getSemantics($item)][] = $name;
       }

       return $groups;
   }

   /**
    * Formats table rows
    */
   function buildTableRows(array $groups): array {
       $max = max(count($groups['success']), count($groups['process']), count($groups['failure']));

       $success = array_pad($groups['success'], $max, '');
       $process = array_pad($groups['process'], $max, '');
       $failure = array_pad($groups['failure'], $max, '');

       $rows = [];
       for ($i = 0; $i < $max; $i++) {
           $rows[] = [
               htmlspecialchars($success[$i]),
               htmlspecialchars($process[$i]),
               htmlspecialchars($failure[$i])
           ];
       }
       return $rows;
   }

   // Leads keep the semantics in EXTRA, the smart process — in SEMANTICS
   $entities = [
       ['title' => 'Lead stages', 'entityId' => 'STATUS'],
       ['title' => 'Smart process stages', 'entityId' => 'DYNAMIC_177_STAGE_7']
   ];

   foreach ($entities as $entity) {
       try {
           $statuses = loadStatuses($sb, $entity['entityId']);
           if (empty($statuses)) {
               echo "<p>No stages for " . htmlspecialchars($entity['title']) . "</p>\n";
               continue;
           }

           $rows = buildTableRows(groupStatusesBySemantics($statuses));

           echo "<h2>" . htmlspecialchars($entity['title']) . "</h2>\n";
           echo "<table border=\"1\" style=\"border-collapse: collapse; width: 100%;\">\n";
           echo "<thead><tr>
               <th style=\"padding: 8px; background: #d4edda;\">Success</th>
               <th style=\"padding: 8px; background: #fff3cd;\">In progress</th>
               <th style=\"padding: 8px; background: #f8d7da;\">Failure</th>
           </tr></thead>\n<tbody>";

           foreach ($rows as $row) {
               echo "<tr>
                   <td style=\"padding: 6px;\">{$row[0]}</td>
                   <td style=\"padding: 6px;\">{$row[1]}</td>
                   <td style=\"padding: 6px;\">{$row[2]}</td>
               </tr>\n";
           }
   
           echo "</tbody></table><br>\n";

       } catch (\Throwable $e) {
           echo "<p style=\"color: red;\">Error: " . htmlspecialchars($e->getMessage()) . "</p>\n";
       }
   }
   ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    # Short semantics to text: there are only three of these values
    SEMANTICS_MAP = {"S": "success", "F": "failure"}

    def load_statuses(client, entity_id: str) -> list:
        return client.crm.status.list(
            filter={"ENTITY_ID": entity_id},
            order={"SORT": "ASC"},
        ).response.result

    def get_semantics(item: dict) -> str:
        """Returns the stage semantics for any CRM object.

        EXTRA is present only for leads, deals, and quotes, so for the other
        objects we convert the short SEMANTICS value into a text one.
        """
        extra = (item.get("EXTRA") or {}).get("SEMANTICS")
        if extra:
            return extra
        return SEMANTICS_MAP.get(item.get("SEMANTICS"), "process")

    def group_statuses_by_semantics(statuses: list) -> dict:
        groups = {"success": [], "process": [], "failure": []}
        for item in statuses:
            name = item.get("NAME") or item.get("STATUS_ID")
            groups[get_semantics(item)].append(name)
        return groups

    def build_table_rows(groups: dict) -> list:
        max_len = max(len(groups["success"]), len(groups["process"]), len(groups["failure"]))

        success = groups["success"] + [""] * (max_len - len(groups["success"]))
        process = groups["process"] + [""] * (max_len - len(groups["process"]))
        failure = groups["failure"] + [""] * (max_len - len(groups["failure"]))

        return [[success[i], process[i], failure[i]] for i in range(max_len)]

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    # Leads keep the semantics in EXTRA, the smart process — in SEMANTICS
    entities = [
        {"title": "Lead stages", "entity_id": "STATUS"},
        {"title": "Smart process stages", "entity_id": "DYNAMIC_177_STAGE_7"},
    ]

    for entity in entities:
        try:
            statuses = load_statuses(client, entity["entity_id"])
        except BitrixAPIError as error:
            print(f"Loading error: {error}")
            continue

        if not statuses:
            print(f"No stages for {entity['title']}")
            continue

        print(entity["title"])
        print("Success\tIn progress\tFailure")
        for row in build_table_rows(group_statuses_by_semantics(statuses)):
            print("\t".join(row))
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/status/crm-status-list.md)
- [{#T}](../../../api-reference/crm/status/crm-status-fields.md)
- [{#T}](../../../api-reference/crm/universal/category/crm-category-list.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md)
- [{#T}](./how-to-get-deal-funnels.md)
