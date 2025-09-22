# How to Get a List of Activities

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

The example retrieves a list of incomplete activities for a contact. To get activities for other objects, replace the value in the `OWNER_TYPE_ID` field. A list of possible values for this field can be obtained using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md).

{% note info %}

To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../first-steps/how-to-use-examples.md)

{% endnote %}

{% list tabs %}

- JS

    ```js
    var contactID = 1;
    var resultActivity = [];

    BX24.callMethod(
        "crm.activity.list",
        {
            filter: {
                COMPLETED: "N", //only new activity
                OWNER_ID: contactID,
                OWNER_TYPE_ID: 3 // CRest::call('crm.enum.ownertype');
            },
            select: [
                "*",
                "COMMUNICATIONS"
            ]
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
    $contactID = 1;
    $resultActivity = [];
    $resultActivity = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'COMPLETED' => 'N',//only new activity
                'OWNER_ID' => $contactID,
                'OWNER_TYPE_ID' => 3, // CRest::call('crm.enum.ownertype');
            ],
            'select' => [
                '*',
                'COMMUNICATIONS'
            ]
        ]
    );
    echo '<pre>';
        print_r($resultActivity);
    echo '</pre>';
    ```

{% endlist %}