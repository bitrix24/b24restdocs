# Event of Call Status Change CallCard::CallStateChanged

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- clarify permissions and scope
- added a custom block "Event Subscription"
- custom block "What the handler receives"

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `CallCard::CallStateChanged` occurs when the status of the current call changes.

## What the handler receives

The specified arguments are passed to the handler.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER***
[`callState`](../../../data-types.md) |  Current state of the call (`idle`, `connecting`, `connected`) ||
|| **additionalParams**
[`object`](../../../data-types.md) | Object with additional fields ||
|#

### Parameter data[]

#|
|| **Parameter**
`type` | **Description** ||
|| **failedCode**
[`string`](../../../data-types.md) | Call completion code. Passed only in case of an unsuccessful call completion when transitioning to the `idle` state ||
|#

## Event Subscription

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"CallCard::CallStateChanged","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.bindEvent"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"CallCard::CallStateChanged","HANDLER":"**your_handler_url_here**"}' \
    "https://**put_your_bitrix24_address**/rest/placement.bindEvent?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.bindEvent("CallCard::CallStateChanged", function (callState) {
        console.log(callState);
    });
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bindEvent',
        [
            'PLACEMENT' => 'CallCard::CallStateChanged',
            'HANDLER' => '**your_handler_url_here**'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./get-status.md)
- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)