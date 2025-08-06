# Call List: Overview of Methods

A call list is a CRM activity that allows you to call multiple clients in succession. An employee works with a single activity that contains the selected contacts or companies. The results of the calls will be automatically saved in the client cards:
- in the attached call activity and its comments,
- in the filled CRM form and fields of the card,
- in the deal or invoice created from the call list.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Calling in Bitrix24](https://helpdesk.bitrix24.com/open/21815426/)

## Connection with Other Objects

**Contact**. To call multiple contacts, specify an array of `ID` of the contacts in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of contacts using the [crm.item.list](../../crm/universal/crm-item-list.md) method with the parameter `entityTypeId = 3`.

**Company**. To call multiple companies, specify an array of `ID` of the companies in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of companies using the [crm.item.list](../../crm/universal/crm-item-list.md) method with the parameter `entityTypeId = 4`.

**CRM Form**. During the call, the employee can fill in information about the client in the CRM form attached to the call list. After the call is completed, the form will be available in the client card, and the information from the form will be saved in the fields of the card. To attach a form to the call list, specify its `ID` in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. The `ID` of the form can be found in the list of forms in Bitrix24 at `https://your-domain.com/crm/webform/`.

**Directory**. The status of the call in the call list card is an element of the [directory](../status/index.md). To change the statuses of the call list, use the directory methods [crm.status.*](../status/index.md) with the parameter `ENTITY_ID = CALL_LIST`.

**User**. The call list is associated with a user by a numeric identifier in the `createdBy` field — who created it. You can obtain user data by identifier using the [user.get](../../user/user-get.md) method.

## How to Delete a Call List

There is no delete method among the call list methods. You can delete a call list by deleting the activity using the [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md) method.

1. Use the [crm.activity.list](../timeline/activities/activity-base/crm-activity-list.md) method with a filter by the activity name `SUBJECT`. In the field value, specify `Call #ID`. Replace `ID` with the `ID` of the call list obtained from the [crm.calllist.add](./crm-calllist-add) or [crm.calllist.list](./crm-calllist-list) methods.

2. Pass the `ID` of the activity from the result to the delete method [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md).

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "crm.activity.list",
        {
            filter: {
                "SUBJECT": "Call #13",
            },
            select: ["ID"]
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            let deals = result.data();

            // If there are no activities, exit
            if (deals.length === 0) {
                console.log("No activities with the name 'Call #13' found.");
                return;
            }

            // Get the ID of the first activity
            let dealId = deals[0].ID;

            // Call the delete method
            BX24.callMethod(
                'crm.activity.delete',
                {
                    id: dealId,
                },
                function(deleteResult) {
                    if (deleteResult.error()) {
                        console.error("Error deleting activity ID " + dealId + ": " + deleteResult.error());
                    } else {
                        console.log("Activity ID " + dealId + " successfully deleted.");
                    }
                }
            );
        }
    );
    ```

- PHP
  
    ```php
    <?php
    require_once('crest.php');

    // 1. Get the list of activities by name
    $result = CRest::call(
        'crm.deal.list',
        [
            'filter' => [
                'TITLE' => 'Call #13'
            ],
            'select' => ['ID']
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    $deals = $result['result'];

    // If there are no activities, exit
    if (empty($deals)) {
        echo "No activities with the name 'Call #13' found.\n";
        exit;
    }

    // Get the ID of the first activity
    $dealId = $deals[0]['ID'];

    // 2. Delete the activity by ID
    $result = CRest::call(
        'crm.deal.delete',
        [
            'id' => $dealId
        ]
    );

    if ($result['error']) {
        echo "Error deleting activity ID $dealId: " . $result['error_description'] . "\n";
    } else {
        echo "Activity ID $dealId successfully deleted.\n";
    }
    ```

{% endlist %}


## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: user with read access to CRM elements

#|
|| **Method** | **Description** ||
|| [crm.calllist.add](./crm-calllist-add.md) | Creates a new call list ||
|| [crm.calllist.get](./crm-calllist-get.md) | Returns information about the call list ||
|| [crm.calllist.items.get](./crm-calllist-items-get.md) | Returns participants of the call list ||
|| [crm.calllist.list](./crm-calllist-list.md) | Returns a list of all call lists ||
|| [crm.calllist.statuslist](./crm-calllist-statuslist.md) | Returns a list of call statuses ||
|| [crm.calllist.update](./crm-calllist-update.md) | Updates the composition of the call list ||
|#