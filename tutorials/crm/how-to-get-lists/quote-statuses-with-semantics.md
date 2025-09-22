# How to Get a List of Estimate Statuses

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

Example of retrieving all statuses of estimates with semantics.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('crm.status.list', { filter: { ENTITY_ID: 'QUOTE_STATUS' } }, function(resultQuote) {
        if (resultQuote.error()) {
            console.error(resultQuote.error());
        } else {
            if (resultQuote.data().length > 0) {
                var table = document.createElement('table');

                var thead = document.createElement('thead');
                var trHead = document.createElement('tr');
                ['STATUS ID', 'NAME', 'SEMANTICS'].forEach(function(text) {
                    var th = document.createElement('th');
                    th.textContent = text;
                    trHead.appendChild(th);
                });
                thead.appendChild(trHead);
                table.appendChild(thead);

                var tbody = document.createElement('tbody');
                resultQuote.data().forEach(function(item) {
                    var tr = document.createElement('tr');
                    if (item.EXTRA && item.EXTRA.COLOR) {
                        tr.style.color = item.EXTRA.COLOR;
                    }
                    ['STATUS_ID', 'NAME', 'EXTRA.SEMANTICS'].forEach(function(key) {
                        var td = document.createElement('td');
                        td.textContent = key.split('.').reduce(function(acc, k) {
                            return acc && acc[k];
                        }, item);
                        tr.appendChild(td);
                    });
                    tbody.appendChild(tr);
                });
                table.appendChild(tbody);

                document.body.appendChild(table);
            }
        }
    });
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../first-steps/how-to-use-examples.md)

    {% endnote %}

    ```php
    <?
    $resultQuote = CRest::call('crm.status.list', ['filter' => ['ENTITY_ID' => 'QUOTE_STATUS']]);
    if (!empty($resultQuote['result'])):
    ?>
        <table>
            <thead>
            <tr>
                <th>STATUS ID</th>
                <th>NAME</th>
                <th>SEMANTICS</th>
            </tr>
            </thead>
            <tbody>
            <? foreach ($resultQuote['result'] as $item): ?>
            <tr <?=(!empty($item['EXTRA']['COLOR']) ? ' style="color:' . $item['EXTRA']['COLOR'] . '"' : '');?>>
                <td><?=$item['STATUS_ID']?></td>
                <td><?=$item['NAME']?></td>
                <td><?=$item['EXTRA']['SEMANTICS']?></td>
            <tr>
                <? endforeach; ?>
            </tbody>
        </table>
    <? endif; ?>
    ```

{% endlist %}