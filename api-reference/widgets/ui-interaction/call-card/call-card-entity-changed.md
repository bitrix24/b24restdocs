# Event of Client Change CallCard::EntityChanged

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- clarify permissions and scope
- added a custom block "Event Subscription"
- custom block "What the handler receives"
- required parameters for transmission are not specified

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The `CallCard::EntityChanged` event occurs when the current client changes during a call review.

## What the Handler Receives

The event handler receives an object with the fields specified below.

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | Client's phone number ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the CRM entity associated with the call (`CONTACT`, `LEAD`, `COMPANY`) ||
|| **CRM_ENTITY_ID**
[`int`](../../../data-types.md) | ID of the CRM entity associated with the call ||
|#

## Event Subscription

{% include [Footnote on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"CallCard::EntityChanged","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.bindEvent"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"CallCard::EntityChanged","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent("CallCard::EntityChanged", function (callState) {
        console.log(callState);
    });
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bindEvent',
        [
            'PLACEMENT' => 'CallCard::EntityChanged',
            'HANDLER' => '**your_handler_url_here**'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Your Exploration

- [{#T}](./get-status.md)
- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)