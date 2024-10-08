# Add ai.prompt.register

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`ai_admin`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `ai.prompt.register` adds a prompt.

#|
|| **Parameter** | **Description** ||
|| **category**
[`unknown`](../../data-types.md) | The category of the embedding location. CoPilot can be located in various places within the product. A list of categories is provided [below](#category) ||
|| **code**
[`unknown`](../../data-types.md) | A unique code for the prompt. The code must use the prefix `rest_`. This code is set once during registration and cannot be changed later.

The pre-prompt can only be updated through its [removal](./ai-prompt-unregister.md) ||
|| **icon**
[`unknown`](../../data-types.md) | Icon code. You can find a suitable icon in the file [copilot_icons.pdf](/upload/api_help/rest/copilot_icons.pdf) ||
|| **prompt**
[`unknown`](../../data-types.md) | The text of the prompt. This is what will be sent to the neural network (the user will not see it). The pre-prompt text can use special formatting: [markers](./markers.md) and [conditions](./conditions.md) ||
|| **translate**
[`unknown`](../../data-types.md) | An array of translations for the item in different languages. Ideally, support at least two languages: English (en) and the account language. Support for other languages is at your discretion. ||
|| **parent_code**
[`unknown`](../../data-types.md) | The code of the parent section. The code must use the prefix `rest_` ||
|| **sort**
[`unknown`](../../data-types.md) | Sorting, specified optionally. Responsible for sorting items among themselves. ||
|| **section**
[`unknown`](../../data-types.md) | The category in the prompt menu for visual separation. It can have the following values:

- create — "Create from text" category
- edit — "Edit text" category

If nothing is specified, the prompt will be placed above these categories.
||
|#

## Category

#|
|| **Embedding Location Category** | **Where it will appear in CoPilot** ||
|| **livefeed** | News feed post ||
|| **livefeed_comments** | Comment on one of the news feed posts ||
|| **tasks** | Task description ||
|| **tasks_comments** | Comment on one of the tasks ||
|| **chat** | Message in one-on-one or group chats ||
|| **mail** | Emails from the personal inbox ||
|| **mail_crm** | Emails with CRM clients in the deals, contacts, companies cards ||
|| **landing** | Texts in the B24 website builder ||
|#

## Examples

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

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

    // example for PHP

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Success Response

## Error Response

## Typical Use-Cases and Scenarios

- [{#T}](../../../tutorials/ai/add-joke-prompt.md)