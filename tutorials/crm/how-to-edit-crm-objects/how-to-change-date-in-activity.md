# How to Change the Scheduled Activity Time

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

The tutorial has been removed from the menu. It needs to be redone, crm.activity.update is not relevant.

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

Example of changing the scheduled activity time to tomorrow, starting at the same time and ending 2 hours later.

{% list tabs %}

- JS

    ```javascript
    let activityID = 42;
    let timeStart = Math.floor(Date.now() / 1000) + 86400; // tomorrow
    let timeEnd = timeStart + 7200; // tomorrow plus 2 hours

    BX24.callMethod(
        "crm.activity.update",
        {
            id: activityID,
            fields: {
                "START_TIME": new Date(timeStart * 1000).toISOString().slice(0, 19).replace('T', ' '),
                "END_TIME": new Date(timeEnd * 1000).toISOString().slice(0, 19).replace('T', ' ')
            }
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../first-steps/how-to-use-examples.md)

    {% endnote %}

    ```php
    <?php
    $activityID = 42;
    $timeStart = time() + 86400; // tomorrow
    $timeEnd = time() + 86400 + 7200; // tomorrow plus 2 hours
    CRest::call(
        'crm.activity.update',
        [
            'id' => $activityID,
            'fields' => [
                "START_TIME" => date("Y-m-d H:i:s", $timeStart),
                "END_TIME" => date("Y-m-d H:i:s", $timeEnd),
            ]
        ]
    );
    ?>
    ```

{% endlist %}