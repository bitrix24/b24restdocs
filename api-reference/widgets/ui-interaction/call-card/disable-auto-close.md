# Disable Auto-Close disableAutoClose

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- clarify permissions and scope
- missing "Response Handling" block
- missing "Error Handling" block

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method disables the automatic closing of the card for 60 seconds after the call ends. It also blocks incoming calls until the card is closed.

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Calling the placement method. The result is returned in the callback.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"disableAutoClose","PARAMS":{}}' \
    "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.call"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"disableAutoClose","PARAMS":{}}' \
    "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call('disableAutoClose', {}, function (result) {
        console.log(result);
    });
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.call',
        [
            'PLACEMENT' => 'disableAutoClose',
            'PARAMS' => (object)[]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./get-status.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)