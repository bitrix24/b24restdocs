# How to Add a Comment to the Timeline of a Smart Process

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The key parameter for adding a comment to a CRM object is the [object type identifier](../../../api-reference/crm/data-types.md#object_type). This identifier indicates which type of object the comment will be added to: a deal, a lead, or a specific smart process. The identifier is used in the parameters `OWNER_TYPE`, `OWNER_TYPE_ID`, `ENTITY_TYPE`, and `ENTITY_TYPE_ID` of the method groups [crm.item.*](../../../api-reference/crm/universal/index.md), [crm.timeline.*](../../../api-reference/crm/timeline/index.md), and [crm.activity.*](../../../api-reference/crm/timeline/activities/index.md).

In CRM, there are two types of object identifiers:
* **Predefined** — these are identifiers for [leads](../../../api-reference/crm/leads/index.md), [deals](../../../api-reference/crm/deals/index.md), [companies](../../../api-reference/crm/companies/index.md), [contacts](../../../api-reference/crm/contacts/index.md), [invoices](../../../api-reference/crm/universal/invoice.md), and [estimates](../../../api-reference/crm/quote/index.md). The identifiers for predefined objects can be found in the [documentation](../../../api-reference/crm/data-types.md#object_type).
* **Dynamic** — these are identifiers for smart processes. The identifier for a smart process is generated at the time of creation and does not depend on the name of the smart process.

You can obtain the identifier for a smart process using two methods:
* [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) — a method without parameters that returns an enumeration of CRM object types, both predefined and dynamic.
* [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — a method with a filter that returns only dynamic CRM objects.

To create a comment in a smart process entity, we will sequentially execute two methods:
1. [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — retrieve the smart process using a filter.
2. [crm.timeline.comment.add](../../../api-reference/crm/timeline/comments/crm-timeline-comment-add.md) — create the comment.

## 1. Retrieve the Smart Process Type Identifier

To obtain the type identifier, we use the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method with a filter:
* `title` — specify the name of the smart process.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'crm.type.list',
        params: {
            filter: {
                "title": "Equipment procurement"
            }
        },
        requestId: 'type-list'
    });
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $sb->getCRMScope()->type()->list(
        order: [],
        filter: ['title' => 'Equipment procurement']
    );
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

    response = client.crm.type.list(
        filter={
            "title": "Equipment procurement",
        }
    ).response
    ```

{% endlist %}

As a result, we obtained two ID values:
* `id`: `7` — the sequential number of the SPA in Bitrix
* `entityTypeId`: `177` — the SPA type identifier. This parameter is required for the next request
  
```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment procurement",
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
                "createdTime": "2021-11-26T10:52:17+03:00",
                "updatedTime": "2024-11-12T15:32:39+03:00",
                "updatedBy": 1
            }
        ]
    }
}
```

## 2. Add a Comment to the Smart Process Entity

To add a comment, use the [crm.timeline.comment.add](../../../api-reference/crm/timeline/comments/crm-timeline-comment-add.md) method with the following parameters:
* `ENTITY_ID` — the item ID. To retrieve the ID value, use the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method, where the `entityTypeId` filter equals the `entityTypeId` value from [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md)
* `ENTITY_TYPE` — specify `DYNAMIC_177`. The value consists of `entityTypeId` from the previous method's result and the dynamic object prefix `DYNAMIC_`
* `COMMENT` — the text value of the comment

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'crm.timeline.comment.add',
        params: {
            fields:
            {
                "ENTITY_ID": 19,
                "ENTITY_TYPE": "DYNAMIC_177",
                "COMMENT": "Confirm the purchase via email!",
            }
        },
        requestId: 'comment-add'
    });
    ```

- PHP

    ```php
    $result = $sb->getCRMScope()->timelineComment()->add(
        [
            'ENTITY_ID' => 19,
            'ENTITY_TYPE' => 'DYNAMIC_177',
            'COMMENT' => 'Confirm the purchase via email!',
        ]
    );
    ```

- Python

    ```python
    response = client.crm.timeline.comment.add(
        fields={
            "ENTITY_ID": 19,
            "ENTITY_TYPE": "DYNAMIC_177",
            "COMMENT": "Confirm the purchase via email!",
        }
    ).response
    ```

{% endlist %}

We added a comment to the SPA item timeline and received the timeline entry ID `55771` in the response. The entry ID can be used in the [update](../../../api-reference/crm/timeline/comments/crm-timeline-comment-update.md) and [delete](../../../api-reference/crm/timeline/comments/crm-timeline-comment-delete.md) methods for the comment.

```json
{
    "result": 55771
}
```

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Function to search for the smart process ID
    async function findSPA() {
        // Smart process name to obtain entityTypeId
        var SPAtitle = 'your_smart_process_name';

        try {
            // Calling the crm.type.list method to obtain entityTypeId
            const result = await $b24.actions.v2.call.make({
                method: 'crm.type.list',
                params: { filter: { title: SPAtitle } },
                requestId: 'type-list'
            });

            var types = result.getData().result.types;
            if (Array.isArray(types) && types.length > 0) {
                var SPAId = types[0].entityTypeId; // Assuming the required object is the first in the array
                console.log('Smart process found', SPAId);
                await createComment(SPAId);
            } else {
                console.error('Smart process not found or data is empty');
            }
        } catch (error) {
            console.error('Error searching for the smart process:', error);
        }
    }

    // Function to create a comment in a smart process element
    async function createComment(SPAId) {
        // Element ID where the comment will be added
        var elementId = 'your_element_ID';
        // Comment text
        var commentText = 'your_comment';

        try {
            // Calling the crm.timeline.comment.add method to add a comment
            const result = await $b24.actions.v2.call.make({
                method: 'crm.timeline.comment.add',
                params: {
                    fields: {
                        ENTITY_ID: elementId,
                        ENTITY_TYPE: 'DYNAMIC_' + SPAId,
                        COMMENT: commentText
                    }
                },
                requestId: 'comment-add'
            });
            console.log('Comment added', result.getData().result);
        } catch (error) {
            console.error('Error creating the comment:', error);
        }
    }

    // Calling the function to search for the smart process and add a comment
    findSPA();
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Services\ServiceBuilder;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function to search for the smart process ID
    function findSPA(ServiceBuilder $sb) {
        // Smart process name to obtain entityTypeId
        $SPAtitle = 'your_smart_process_name';

        try {
            // Calling the crm.type.list method to obtain entityTypeId
            $types = $sb->getCRMScope()->type()->list(
                order: [],
                filter: ['title' => $SPAtitle]
            )->getTypes();

            if (is_array($types) && count($types) > 0) {
                $SPAId = $types[0]->entityTypeId; // Assuming the required object is the first in the array
                echo 'Smart process found: ' . $SPAId;
                createComment($sb, $SPAId);
            } else {
                echo 'Smart process not found or data is empty';
            }
        } catch (\Throwable $e) {
            echo 'Error searching for the smart process: ' . $e->getMessage();
        }
    }

    // Function to create a comment in a smart process element
    function createComment(ServiceBuilder $sb, $SPAId) {
        // Element ID where the comment will be added
        $elementId = 'your_element_ID';
        // Comment text
        $commentText = 'your_comment';

        try {
            // Calling the crm.timeline.comment.add method to add a comment
            $sb->getCRMScope()->timelineComment()->add(
                [
                    'ENTITY_ID' => $elementId,
                    'ENTITY_TYPE' => 'DYNAMIC_' . $SPAId,
                    'COMMENT' => $commentText
                ]
            );
            echo 'Comment added';
        } catch (\Throwable $e) {
            echo 'Error creating the comment: ' . $e->getMessage();
        }
    }

    // Calling the function to search for the smart process and add a comment
    findSPA($sb);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def find_spa(client):
        spa_title = "your_smart_process_name"

        try:
            resp = client.crm.type.list(
                filter={"title": spa_title},
            ).response
        except BitrixAPIError as error:
            print(f"Error searching for the smart process: {error}")
            return

        types = resp.result["types"]
        if types:
            spa_id = types[0]["entityTypeId"]
            print(f"Smart process found: {spa_id}")
            create_comment(client, spa_id)
        else:
            print("Smart process not found or data is empty")

    def create_comment(client, spa_id):
        element_id = "your_element_ID"
        comment_text = "your_comment"

        try:
            client.crm.timeline.comment.add(
                fields={
                    "ENTITY_ID": element_id,
                    "ENTITY_TYPE": f"DYNAMIC_{spa_id}",
                    "COMMENT": comment_text,
                },
            ).response
        except BitrixAPIError as error:
            print(f"Error creating the comment: {error}")
        else:
            print("Comment added")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    find_spa(client)
    ```

{% endlist %}
