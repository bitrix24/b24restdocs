# How to Use Attachments

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

Attachments can be applied to any messages (from users or bots) and notifications within the messenger.

You create an **Attachment** object and pass it to the message sending method under the key **ATTACH** (this can be either the **full** or **shortened** form of the attachment).

## Full Version of the ATTACH Object

{% list tabs %}

- JS

    ```js
    ATTACH: {
        ID: 1,
        COLOR_TOKEN: "secondary",
        COLOR: "#29619b",
        BLOCKS: [
            {...},
            {...},
        ]
    }
    ```

- PHP

    ```php
    "ATTACH" => Array(
        "ID" => 1,
        "COLOR_TOKEN" => "secondary",
        "COLOR" => "#29619b",
        "BLOCKS" => Array(
            array(...),
            array(...),
        )
    )
    ```

{% endlist %}

**Array Keys**:
- `ID` — identifier of the block
- `COLOR_TOKEN` — responsible for the color highlighting of the attachment. Can take one of the following values:
  - `primary`
  - `secondary`
  - `alert`
  - `base`
    Defaults to `base`
- `COLOR` — responsible for the color highlighting of the attachment in the chat. Used for backward compatibility in open line chats and notifications. By default, the attachment color is assigned to the recipient's chat color (or, if it's a notification, to the current user's color). This key can be omitted if not needed.
- `BLOCKS` must contain the markup blocks that we will discuss shortly.

## Example:

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
        BX24.callMethod(
        'imbot.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'Attachment with primary color',
            ATTACH: {
                ID: 1,
                COLOR_TOKEN: "primary",
                COLOR: "#29619b",
                BLOCKS: [
                    {
                        MESSAGE: "The API will be available in the update [B]im 24.0.0[/B]"
                    }
                ]
            }
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation of restCommand](../../_includes/rest-command.md) %}

    ```php
    restCommand(
        'imbot.message.add',
        Array(
            "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
            "MESSAGE" => "Attachment with primary color",
            "ATTACH" => Array(
                "ID" => 1,
                "COLOR_TOKEN" => "primary",
                "COLOR" => "#29619b",
                "BLOCKS" => Array(
                    Array(
                        "MESSAGE" => "The API will be available in the update [B]im 24.0.0[/B]"
                    )
                )
            )
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

## Short Version of the ATTACH Object

If you are okay with the attachment being at the bottom of the message and do not need to specify a color, you can use the **short** version:

{% list tabs %}

- JS

    ```js
    ATTACH: [
        {...},
        {...},
    ]
    ```

- PHP

    ```php
    "ATTACH" => Array(
        array(...),
        array(...),
    )
    ```

{% endlist %}

Unlike the full version, the markup blocks are specified directly at the first level without declaring the **BLOCKS** key.

## Example:

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'imbot.message.add',
        {
            DIALOG_ID: 'chat20921',
            MESSAGE: 'Text block',
            ATTACH: [
                {
                    MESSAGE: "The API will be available in the update [B]im 24.0.0[/B]"
                }
            ]
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation of restCommand](../../_includes/rest-command.md) %}

    ```php
    restCommand(
        'imbot.message.add',
        Array(
            "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
            "MESSAGE" => "Text block",
            "ATTACH" => Array(
                Array(
                    "MESSAGE" => "The API will be available in the update [B]im 24.0.0[/B]"
                )
            )
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% note warning %}

Due to the complexity of the structure, attachments are not automatically added when sent via XMPP, email, or as PUSH notifications to a phone.

{% endnote %}