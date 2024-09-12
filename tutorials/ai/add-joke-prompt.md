# Add a New Prompt "Joke"

Registering a new prompt can be done using the method [ai.prompt.register](../../api-reference/ai/prompts/ai-prompt-register.md). Let's look at how to use it by adding a category for jokes.

{% include [Example Notes](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'ai.prompt.register',
        {
            category: [
                "livefeed",
                "livefeed_comments"
            ],
            code: 'rest_joke',
            icon: 'smile',
            prompt: 'Tell a joke',
            translate: {
                "en":"A joke"
            }
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'ai.prompt.register',
        [
            'category' => [
                "livefeed",
                "livefeed_comments"
            ],
            'code' => 'rest_joke',
            'icon' => 'smile',
            'prompt' => 'Tell a joke',
            'translate' => [
                "en" => "A joke"
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

After executing the command, a CoPilot option will appear that can generate jokes.

The "Joke" menu item is now clickable. Let's make it a parent item by adding sub-items (after this, the parent item will no longer be clickable).

To do this, you need to add the parent section code `parent_code` to the original code, as well as `sort` for sorting the items among themselves.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'ai.prompt.register',
        {
            category: [
                "livefeed",
                "livefeed_comments"
            ],
            code: 'rest_joke_wolf',
            icon: 'smile',
            prompt: 'Tell a joke about a wolf',
            translate: {
                "en":"A joke about wolf"
            },
            parent_code: 'rest_joke',
            sort: 100
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'ai.prompt.register',
        [
            'category' => [
                "livefeed",
                "livefeed_comments"
            ],
            'code' => 'rest_joke_wolf',
            'icon' => 'smile',
            'prompt' => 'Tell a joke about a wolf',
            'translate' => [
                "en" => "A joke about wolf"
            ],
            'parent_code' => 'rest_joke',
            'sort' => 100
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

To embed a sub-item into existing items, find out the code of that item and use the previous code. The code can be found by opening the comment on the post in the news feed. At this moment, a command is sent to retrieve information about CoPilot, where everything is shown.

There are also visual categories where you can place your items.

Pass the parameter `section` in the command, which can be either `create` or `edit`. This visual separation indicates whether the pre-prompt changes text or creates it.

## How to Change a Prompt

If you no longer need the pre-prompt or want to change it, execute the command [ai.prompt.unregister](../../api-reference/ai/prompts/ai-prompt-unregister.md):

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'ai.prompt.unregister',
        {
            code: 'rest_joke_wolf'
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'ai.prompt.unregister',
        [
            'code' => 'rest_joke_wolf'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}