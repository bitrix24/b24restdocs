# How to Add a Comment to the Smart Process Timeline

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the CRM entity

The key parameter for adding a comment to a CRM entity is the [object type identifier](../../../api-reference/crm/data-types.md#object_type). The identifier indicates which type of object the comment will be added to: a deal, a lead, or a specific smart process. The identifier is used in the parameters `OWNER_TYPE`, `OWNER_TYPE_ID`, `ENTITY_TYPE`, and `ENTITY_TYPE_ID` of the method groups [crm.item.*](../../../api-reference/crm/universal/index.md), [crm.timeline.*](../../../api-reference/crm/timeline/index.md), [crm.activity.*](../../../api-reference/crm/timeline/activities/index.md).

In CRM, there are two types of object identifiers:  
* **Predefined** — these are identifiers for [leads](../../../api-reference/crm/leads/index.md), [deals](../../../api-reference/crm/deals/index.md), [companies](../../../api-reference/crm/companies/index.md), [contacts](../../../api-reference/crm/contacts/index.md), [invoices](../../../api-reference/crm/universal/invoice.md), and [estimates](../../../api-reference/crm/quote/index.md). The identifiers for predefined objects can be found in the [documentation](../../../api-reference/crm/data-types.md#object_type).
* **Dynamic** — these are identifiers for smart processes. The smart process identifier is generated at the time of creation and does not depend on the name of the smart process.

You can obtain the smart process identifier using two methods:
* [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) — a method without parameters that returns an enumeration of CRM object types, both predefined and dynamic.
* [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — a method with a filter that returns only dynamic CRM objects.

To create a comment in the smart process entity, we will sequentially execute two methods: 
1. [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — retrieve the smart process by filter.
2. [crm.timeline.comment.add](../../../api-reference/crm/timeline/comments/crm-timeline-comment-add.md) — create the comment.

## 1. Retrieve the Smart Process Type Identifier

To obtain the type identifier, we use the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method with a filter:
* `title` — specify the name of the smart process.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.type.list',
        {
            filter: {
                "title": "Equipment Purchase"
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.type.list',
        [
            'filter' => [
                'title' => 'Equipment Purchase'
            ]
        ]
    );
    ```

{% endlist %}

As a result, we obtain two ID values:
* `id`: `7` — the ordinal number of the smart process in Bitrix
* `entityTypeId`: `177` — the identifier of the smart process type. This parameter is necessary for the next request.
  
```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment Purchase",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 177,
                "customSectionId": null,
                "isCategoriesEnabled": "Y",
                "isStagesEnabled": "Y",
                "isBeginCloseDatesEnabled": "Y",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "Y",
                "isMycompanyEnabled": "Y",
                "isDocumentsEnabled": "Y",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "Y",
                "isRecyclebinEnabled": "Y",
                "isAutomationEnabled": "Y",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2021-11-26T10:52:17+02:00",
                "updatedTime": "2024-11-12T15:32:39+02:00",
                "updatedBy": 1
            }
        ]
    }
}
```
## 2. Add a Comment to the Smart Process Entity

To add a comment, we use the [crm.timeline.comment.add](../../../api-reference/crm/timeline/comments/crm-timeline-comment-add.md) method with the following parameters:
* `ENTITY_ID` — the ID of the entity. To obtain the ID value, use the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method, where the `entityTypeId` filter equals the `entityTypeId` value from [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md).
* `ENTITY_TYPE` — specify `DYNAMIC_177`. The value consists of the `entityTypeId` from the result of the previous method and the prefix for dynamic objects `DYNAMIC_`.
* `COMMENT` — the text value of the comment.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "crm.timeline.comment.add",
        {
            fields:
            {
                "ENTITY_ID": 19,
                "ENTITY_TYPE": "DYNAMIC_177",
                "COMMENT": "Confirm the purchase via e-mail!",
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.comment.add',
        [
            'fields' => [
                'ENTITY_ID' => 19,
                'ENTITY_TYPE' => 'DYNAMIC_177',
                'COMMENT' => 'Confirm the purchase via e-mail!',
            ]
        ]
    );
    ```

{% endlist %}

We have added a comment to the timeline of the smart process entity and received the timeline record ID `55771` in response. This record ID can be used in the [update](../../../api-reference/crm/timeline/comments/crm-timeline-comment-update.md) and [delete](../../../api-reference/crm/timeline/comments/crm-timeline-comment-delete.md) methods for comments.

```json
{
    "result": 55771
}
```

## Code Example

{% list tabs %}

- JS

    ```javascript
    // Function to find the smart process identifier
    function findSPA() {
        // Name of the smart process to obtain entityTypeId
        var SPAtitle = 'your_smart_process_name';

        // Call the crm.type.list method to get entityTypeId
        BX.rest.callMethod(
            'crm.type.list',
            {
                filter: {
                    title: SPAtitle
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error finding smart process:', result.error());
                } else {
                    var types = result.data().types;
                    if (Array.isArray(types) && types.length > 0) {
                        var SPAId = types[0].entityTypeId; // Assume the desired object is the first in the array
                        console.log('Smart process found', SPAId);
                        createComment(SPAId);
                    } else {
                        console.error('Smart process not found or data is empty');
                    }
                }
            }
        );
    }

    // Function to create a comment in the smart process entity
    function createComment(SPAId) {
        // ID of the entity to which the comment will be added
        var elementId = 'your_ELEMENT_ID';
        // Text of the comment
        var commentText = 'your_comment';

        // Call the crm.timeline.comment.add method to add the comment
        BX.rest.callMethod(
            "crm.timeline.comment.add",
            {
                fields: {
                    ENTITY_ID: elementId,
                    ENTITY_TYPE: 'DYNAMIC_' + SPAId,
                    COMMENT: commentText
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error creating comment:', result.error());
                } else {
                    console.log('Comment added', result.data());
                }
            }
        );
    }

    // Call the function to find the smart process and add a comment
    findSPA();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to find the smart process identifier
    function findSPA() {
        // Name of the smart process to obtain entityTypeId
        $SPAtitle = 'your_smart_process_name';

        // Call the crm.type.list method to get entityTypeId
        $result = CRest::call(
            'crm.type.list',
            [
                'filter' => [
                    'title' => $SPAtitle
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error finding smart process: ' . $result['error'];
        } else {
            $types = $result['result']['types'];
            if (is_array($types) && count($types) > 0) {
                $SPAId = $types[0]['entityTypeId']; // Assume the desired object is the first in the array
                echo 'Smart process found: ' . $SPAId;
                createComment($SPAId);
            } else {
                echo 'Smart process not found or data is empty';
            }
        }
    }

    // Function to create a comment in the smart process entity
    function createComment($SPAId) {
        // ID of the entity to which the comment will be added
        $elementId = 'your_ELEMENT_ID';
        // Text of the comment
        $commentText = 'your_comment';

        // Call the crm.timeline.comment.add method to add the comment
        $result = CRest::call(
            'crm.timeline.comment.add',
            [
                'fields' => [
                    'ENTITY_ID' => $elementId,
                    'ENTITY_TYPE' => 'DYNAMIC_' . $SPAId,
                    'COMMENT' => $commentText
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error creating comment: ' . $result['error'];
        } else {
            echo 'Comment added';
        }
    }

    // Call the function to find the smart process and add a comment
    findSPA();
    ```

{% endlist %}