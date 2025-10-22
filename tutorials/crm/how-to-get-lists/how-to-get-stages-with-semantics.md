# How to Get a List of Stages with Semantics for CRM Objects

> Scope: [`crm, user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user with access to CRM

The semantics of a stage reflects the current state of a CRM entity: in progress, successfully completed, or unsuccessful. The system uses the semantics value in automation and reporting.

To create a table of stages for a CRM object with semantics, we use the method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md).

## Get a List of Stages with Semantics

The method [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) returns a description of stages by the `ENTITY_ID` stage code for the CRM object.

-  [Deals](../../../api-reference/crm/deals/index.md) — `DEAL_STAGE` for the main direction of deals and `DEAL_STAGE_xx` for additional ones, where xx is the direction identifier.

-  [Leads](../../../api-reference/crm/leads/index.md) —  `STATUS`.

-  [Invoices](../../../api-reference/crm/universal/invoice.md) — `SMART_INVOICE_STAGE_xx`, where `xx` is the invoice direction identifier value.

-  [Estimates](../../../api-reference/crm/quote/index.md) — `QUOTE_STATUS`.

-  [Documents](https://helpdesk.bitrix24.com/open/19441484/) — `SMART_DOCUMENT_STAGE_xx`, where `xx` is the `ID` of the document direction.

-  [SPAs](../../../api-reference/crm/universal/index.md) —  `DYNAMIC_xx_STAGE_xx`, where the first `xx` is the `ID` of the SPA, and the second `xx` is the direction `ID`.

We will get a description of stages with semantics for leads. To do this, we specify in the filter `filter` the field `ENTITY_ID` with the value `STATUS`.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

    ```javascript
    BX24.callMethod(
        "crm.status.list",
        {
            order: { SORT: "ASC" }, // sorting in ascending order by the SORT field value
            filter: { ENTITY_ID: "STATUS" }, // getting stages for leads
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

-  PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.status.list',
        [
            'order' => [ 'SORT' => 'ASC' ],
            'filter' => [ 'ENTITY_ID' => 'STATUS' ]
        ]
    );
    ```

{% endlist %}

As a result, we will get an array of objects, where each object is a description of a stage.

```json
{
    "result": [
        {
            "ID": "1",
            "ENTITY_ID": "STATUS",
            "STATUS_ID": "NEW",
            "NAME": "Not Processed",
            "NAME_INIT": "Not Processed",
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
            "ID": "3",
            "ENTITY_ID": "STATUS",
            "STATUS_ID": "ASSIGNED",
            "NAME": "Responsible Assigned",
            "NAME_INIT": "",
            "SORT": "20",
            "SYSTEM": "N",
            "CATEGORY_ID": null,
            "COLOR": "#FFF100",
            "SEMANTICS": null,
            "EXTRA": {
                "SEMANTICS": "process",
                "COLOR": "#FFF100"
            }
        },
        ...,
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
            "NAME": "Poor Quality Lead",
            "NAME_INIT": "Poor Quality Lead",
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

The `EXTRA.SEMANTICS` object contains the semantics of the stages. Possible values:

-  `process` — the CRM entity is in progress,

-  `success` — work with the CRM entity has been successfully completed,

-  `failure` — work with the CRM entity has been unsuccessfully completed.

## Code Example

The code outputs tables with the list of stages for leads and estimates.

{% list tabs %}

-  JS

   ```javascript
   /**
    * Loads all statuses for the given ENTITY_ID
    * @param {string} entityId — entity code, e.g., 'STATUS' or 'QUOTE_STATUS'
    * @returns {Promise<Array>} — array of all statuses
    */
   function loadStatuses(entityId) {
       return new Promise((resolve, reject) => {
           BX24.callMethod('crm.status.list', {
               filter: { ENTITY_ID: entityId },
               select: ['STATUS_ID', 'NAME', 'EXTRA'],
               order: { SORT: 'ASC' }
           }, (result) => {
               if (result.error()) {
                   reject(result.error());
                   return;
               }
               resolve(result.data());
           });
       });
   }
   
   /**
    * Groups statuses by semantics
    */
   function groupStatusesBySemantics(statuses) {
       const groups = { success: [], process: [], failure: [] };
   
       statuses.forEach(item => {
           const semantics = item.EXTRA?.SEMANTICS || '';
           const name = item.NAME || item.STATUS_ID;
   
           if (semantics === 'success') {
               groups.success.push(name);
           } else if (semantics === 'failure') {
               groups.failure.push(name);
           } else {
               groups.process.push(name);
           }
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
           '✅ Success': pad(success, maxLen)[i],
           '⚠️ In Progress': pad(process, maxLen)[i],
           '❌ Failure': pad(failure, maxLen)[i]
       }));
   }
   
   // Requesting statuses
   Promise.all([
       loadStatuses('STATUS').then(data => ({ type: 'Leads', data })),
       loadStatuses('QUOTE_STATUS').then(data => ({ type: 'Estimates', data }))
   ]).then(results => {
       results.forEach(({ type, data }) => {
           console.group(`📊 ${type}`);
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
   require_once 'crest.php';
   
   /**
    * Gets all statuses for the given ENTITY_ID
    * @param string $entityId
    * @return array
    */
   function loadStatuses($entityId) {
       $result = CRest::call('crm.status.list', [
           'filter' => ['ENTITY_ID' => $entityId],
           'select' => ['STATUS_ID', 'NAME', 'EXTRA'],
           'order'  => ['SORT' => 'ASC']
       ]);
   
       if (!empty($result['error'])) {
           throw new Exception("Error loading statuses $entityId: " . $result['error_description']);
       }
   
       return $result['result'];
   }
   
   /**
    * Groups statuses by semantics
    */
   function groupStatusesBySemantics($statuses) {
       $groups = ['success' => [], 'process' => [], 'failure' => []];
   
       foreach ($statuses as $item) {
           $semantics = $item['EXTRA']['SEMANTICS'] ?? '';
           $name = $item['NAME'] ?? $item['STATUS_ID'];
   
           if ($semantics === 'success') {
               $groups['success'][] = $name;
           } elseif ($semantics === 'failure') {
               $groups['failure'][] = $name;
           } else {
               $groups['process'][] = $name;
           }
       }
   
       return $groups;
   }
   
   /**
    * Formats table rows
    */
   function buildTableRows($groups) {
       $success = $groups['success'];
       $process = $groups['process'];
       $failure = $groups['failure'];
       $max = max(count($success), count($process), count($failure));
   
       $success = array_pad($success, $max, '');
       $process = array_pad($process, $max, '');
       $failure = array_pad($failure, $max, '');
   
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
   
   $entities = [
       ['title' => 'Lead Statuses', 'entityId' => 'STATUS'],
       ['title' => 'Estimate Statuses', 'entityId' => 'QUOTE_STATUS']
   ];
   
   foreach ($entities as $entity) {
       try {
           $statuses = loadStatuses($entity['entityId']);
           if (empty($statuses)) {
               echo "<p>No statuses for " . htmlspecialchars($entity['title']) . "</p>\n";
               continue;
           }
   
           $groups = groupStatusesBySemantics($statuses);
           $rows = buildTableRows($groups);
   
           echo "<h2>" . htmlspecialchars($entity['title']) . "</h2>\n";
           echo "<table border=\"1\" style=\"border-collapse: collapse; width: 100%;\">\n";
           echo "<thead><tr>
               <th style=\"padding: 8px; background: #d4edda;\">✅ Success</th>
               <th style=\"padding: 8px; background: #fff3cd;\">⚠️ In Progress</th>
               <th style=\"padding: 8px; background: #f8d7da;\">❌ Failure</th>
           </tr></thead>\n<tbody>";
   
           foreach ($rows as $row) {
               echo "<tr>
                   <td style=\"padding: 6px;\">{$row[0]}</td>
                   <td style=\"padding: 6px;\">{$row[1]}</td>
                   <td style=\"padding: 6px;\">{$row[2]}</td>
               </tr>\n";
           }
   
           echo "</tbody></table><br>\n";
   
       } catch (Exception $e) {
           echo "<p style=\"color: red;\">Error: " . htmlspecialchars($e->getMessage()) . "</p>\n";
       }
   }
   ```

{% endlist %}