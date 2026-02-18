# Call List: Overview of Methods

A call list is a CRM activity from which multiple clients can be called consecutively. An employee works with a single activity that contains selected contacts or companies. The results of the calls will be automatically saved in the client cards:
- in the attached call activity and its comments,
- in the filled CRM form and fields of the card,
- in the deal or invoice created from the call list.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Calling in Bitrix24](https://helpdesk.bitrix24.com/open/21815426/)

## Connection with Other Objects

**Contact**. To call multiple contacts, specify an array of `ID` contacts in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of contacts using the [crm.item.list](../../crm/universal/crm-item-list.md) method with the parameter `entityTypeId = 3`.

**Company**. To call multiple companies, specify an array of `ID` companies in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of companies using the [crm.item.list](../../crm/universal/crm-item-list.md) method with the parameter `entityTypeId = 4`.

**CRM Form**. During the call, the employee can fill in information about the client in the CRM form attached to the call list. After the call is completed, the form will be available in the client card, and the information from the form will be saved in the fields of the card. To attach a form to the call list, specify its `ID` in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. The `ID` of the form can be found in the list of forms in Bitrix24 at `https://your-domain.com/crm/webform/`.

**Directory**. The status of the call in the call list card is an element of the [directory](../status/index.md). To change the statuses of the call list, use the directory methods [crm.status.*](../status/index.md) with the parameter `ENTITY_ID = CALL_LIST`.

**User**. The call list is linked to a user by a numeric identifier in the `createdBy` field — who created it. You can obtain user data by identifier using the [user.get](../../user/user-get.md) method.

## How to Delete a Call List

There is no method for deleting a call list among the call list methods. You can delete a call list by deleting the activity using the [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md) method.

1. Use the [crm.activity.list](../timeline/activities/activity-base/crm-activity-list.md) method with a filter by the activity name `SUBJECT`. In the field value, specify `Call #ID`. Replace `ID` with the `ID` of the call list obtained from the [crm.calllist.add](./crm-calllist-add) or [crm.calllist.list](./crm-calllist-list) methods.

2. Pass the `ID` of the activity from the result to the deletion method [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md).

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'crm.activity.list',
        {
          filter: {
            "SUBJECT": "Call #13",
          },
          select: ["ID"]
        }
      )
      const deals = response.getData() || []
    
      if (deals.length === 0) {
        console.log("No activities found with the name 'Call #13'.");
        return;
      }
    
      let dealId = deals[0].ID;
    
      await $b24.callMethod(
        'crm.activity.delete',
        {
          id: dealId,
        }
      )
      console.log("Activity ID " + dealId + " successfully deleted.");
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('crm.activity.list', {
        filter: {
          "SUBJECT": "Call #13",
        },
        select: ["ID"]
      }, 'ID')
    
      for await (const page of generator) {
        if (page.length === 0) {
          console.log("No activities found with the name 'Call #13'.");
          return;
        }
    
        let dealId = page[0].ID;
    
        await $b24.callMethod(
          'crm.activity.delete',
          {
            id: dealId,
          }
        )
        console.log("Activity ID " + dealId + " successfully deleted.");
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.activity.list', {
        filter: {
          "SUBJECT": "Call #13",
        },
        select: ["ID"]
      }, 0)
    
      const deals = response.getData().result || []
    
      if (deals.length === 0) {
        console.log("No activities found with the name 'Call #13'.");
        return;
      }
    
      let dealId = deals[0].ID;
    
      await $b24.callMethod(
        'crm.activity.delete',
        {
          id: dealId,
        }
      )
      console.log("Activity ID " + dealId + " successfully deleted.");
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.list',
                [
                    'filter' => [
                        'SUBJECT' => 'Call #13',
                    ],
                    'select' => ['ID']
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            return;
        }
    
        $deals = $result->data();
    
        // If there are no activities, exit
        if (count($deals) === 0) {
            echo "No activities found with the name 'Call #13'.";
            return;
        }
    
        // Get the ID of the first activity
        $dealId = $deals[0]['ID'];
    
        // Call the delete method
        $deleteResponse = $b24Service
            ->core
            ->call(
                'crm.activity.delete',
                [
                    'id' => $dealId,
                ]
            );
    
        $deleteResult = $deleteResponse
            ->getResponseData()
            ->getResult();
    
        if ($deleteResult->error()) {
            error_log("Error deleting activity ID " . $dealId . ": " . $deleteResult->error());
        } else {
            echo "Activity ID " . $dealId . " successfully deleted.";
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

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
                console.log("No activities found with the name 'Call #13'.");
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

- PHP CRest

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
        echo "No activities found with the name 'Call #13'.\n";
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
> Who can perform methods: user with permissions to read CRM elements

#|
|| **Method** | **Description** ||
|| [crm.calllist.add](./crm-calllist-add.md) | Creates a new call list ||
|| [crm.calllist.get](./crm-calllist-get.md) | Returns information about the call list ||
|| [crm.calllist.items.get](./crm-calllist-items-get.md) | Returns participants of the call list ||
|| [crm.calllist.list](./crm-calllist-list.md) | Returns a list of all call lists ||
|| [crm.calllist.statuslist](./crm-calllist-statuslist.md) | Returns a list of call statuses ||
|| [crm.calllist.update](./crm-calllist-update.md) | Updates the composition of the call list ||
|#
