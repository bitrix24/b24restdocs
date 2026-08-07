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

- Go

    ```go
    res, err := core.Call(ctx, "crm.type.list", b24.Params{
    	"filter": b24.Params{"title": spaTitle},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.type.list: %w", err)
    }

    // The method wraps the response in an object with the types key. Two smart processes
    // may have identical titles, so the response is a list even with
    // an exact filter.
    var types struct {
    	Types []struct {
    		ID           int    `json:"id"`
    		EntityTypeID int    `json:"entityTypeId"`
    		Title        string `json:"title"`
    	} `json:"types"`
    }
    if err := json.Unmarshal(res.Result, &types); err != nil {
    	return fmt.Errorf("parse smart processes: %w", err)
    }
    if len(types.Types) == 0 {
    	return fmt.Errorf("smart process %q not found", spaTitle)
    }

    // id is the sequential number of the smart process, entityTypeId is the ID of its
    // TYPE. Further on you need exactly entityTypeId, these are different numbers.
    entityTypeID := types.Types[0].EntityTypeID
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

- Go

    ```go
    // ENTITY_TYPE for a smart process is the string "DYNAMIC_" + entityTypeId.
    // Timeline fields are written in UPPERCASE, whereas crm.item.* accepts
    // camelCase: one entity, two conventions in a single scenario.
    res, err = core.Call(ctx, "crm.timeline.comment.add", b24.Params{
    	"fields": b24.Params{
    		"ENTITY_ID":   itemID,
    		"ENTITY_TYPE": "DYNAMIC_" + strconv.Itoa(entityTypeID),
    		"COMMENT":     "Confirm the purchase via email!",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.timeline.comment.add: %w", err)
    }

    // There is no wrapper here at all: result is the ID of the timeline
    // record itself, as a bare number.
    var commentID b24.ID
    if err := json.Unmarshal(res.Result, &commentID); err != nil {
    	return fmt.Errorf("parse comment ID: %w", err)
    }
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

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates a smart process and an item in it, finds
    // the smart process by title, adds a comment to the item timeline, and
    // cleans up after itself. It runs on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"errors"
    	"fmt"
    	"log"
    	"os"
    	"strconv"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // The smart process title is the same one that step 1 looks for.
    const spaTitle = "Equipment procurement (b24gosdk example)"

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: our own smart process and an item in it

    	typeID, err := addType(ctx, core, spaTitle)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.type.delete", b24.Params{"id": typeID})

    	// entityTypeId is needed both to create the item and for the comment, but so far
    	// only the id of the type itself is known — step 1 goes for entityTypeId.

    	// --- step 1: find the smart process by its title
    	res, err := core.Call(ctx, "crm.type.list", b24.Params{
    		"filter": b24.Params{"title": spaTitle},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.type.list: %w", err)
    	}

    	// The method wraps the response in an object with the types key. Two smart processes
    	// may have identical titles, so the response is a list even with
    	// an exact filter.
    	var types struct {
    		Types []struct {
    			ID           int    `json:"id"`
    			EntityTypeID int    `json:"entityTypeId"`
    			Title        string `json:"title"`
    		} `json:"types"`
    	}
    	if err := json.Unmarshal(res.Result, &types); err != nil {
    		return fmt.Errorf("parse smart processes: %w", err)
    	}
    	if len(types.Types) == 0 {
    		return fmt.Errorf("smart process %q not found", spaTitle)
    	}

    	// id is the sequential number of the smart process, entityTypeId is the ID of its
    	// TYPE. Further on you need exactly entityTypeId, these are different numbers.
    	entityTypeID := types.Types[0].EntityTypeID
    	fmt.Printf("smart process %q: id=%d, entityTypeId=%d\n",
    		types.Types[0].Title, types.Types[0].ID, entityTypeID)

    	itemID, err := addItem(ctx, core, entityTypeID, "Laptop procurement")
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.item.delete", b24.Params{
    		"entityTypeId": entityTypeID, "id": itemID,
    	})

    	// --- step 2: add a comment to the item timeline
    	// ENTITY_TYPE for a smart process is the string "DYNAMIC_" + entityTypeId.
    	// Timeline fields are written in UPPERCASE, whereas crm.item.* accepts
    	// camelCase: one entity, two conventions in a single scenario.
    	res, err = core.Call(ctx, "crm.timeline.comment.add", b24.Params{
    		"fields": b24.Params{
    			"ENTITY_ID":   itemID,
    			"ENTITY_TYPE": "DYNAMIC_" + strconv.Itoa(entityTypeID),
    			"COMMENT":     "Confirm the purchase via email!",
    		},
    	})
    	if err != nil {
    		return fmt.Errorf("crm.timeline.comment.add: %w", err)
    	}

    	// There is no wrapper here at all: result is the ID of the timeline
    	// record itself, as a bare number.
    	var commentID b24.ID
    	if err := json.Unmarshal(res.Result, &commentID); err != nil {
    		return fmt.Errorf("parse comment ID: %w", err)
    	}
    	fmt.Printf("comment %d added to item %d\n", commentID, itemID)
    	return nil
    }

    // --- helpers: data setup and cleanup

    // addType creates a smart process. entityTypeId is deliberately not passed: it is
    // issued by the portal, and that is exactly what step 1 goes for.
    func addType(ctx context.Context, core *b24.Core, title string) (b24.ID, error) {
    	// isRecyclebinEnabled is disabled deliberately: an item in the recycle bin still
    	// counts as an item, and crm.type.delete refuses to delete a type
    	// that has items.
    	res, err := core.Call(ctx, "crm.type.add", b24.Params{
    		"fields": b24.Params{"title": title, "isRecyclebinEnabled": "N"},
    	})
    	if err != nil {
    		// On plans without smart processes, the method responds with a dedicated code.
    		// The code is compared with errors.Is rather than as a string: a typo in the literal
    		// would compile and silently take a different branch.
    		if errors.Is(err, b24.Code("CREATE_DYNAMIC_TYPE_RESTRICTED")) {
    			return 0, fmt.Errorf("a smart process cannot be created on this portal: %w", err)
    		}
    		return 0, fmt.Errorf("crm.type.add: %w", err)
    	}
    	raw, ok := b24.Unwrap(res.Result, "type", "id")
    	if !ok {
    		return 0, fmt.Errorf("no type.id in %s", res.Result)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(raw, &id)
    }

    func addItem(ctx context.Context, core *b24.Core, entityTypeID int, title string) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.item.add", b24.Params{
    		"entityTypeId": entityTypeID,
    		"fields":       b24.Params{"title": title},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("crm.item.add: %w", err)
    	}
    	raw, ok := b24.Unwrap(res.Result, "item", "id")
    	if !ok {
    		return 0, fmt.Errorf("no item.id in %s", res.Result)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(raw, &id)
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}
