# Get Call Status getStatus

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- clarify permissions and scope
- the "Response Handling" section lacks an example
- there is no "Error Handling" section

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns information about the current call.

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Calling the placement method. The result is returned in a callback.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"getStatus","PARAMS":{}}' \
    "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.call"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"getStatus","PARAMS":{}}' \
    "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call('getStatus', {}, function (result) {
        console.log(result);
    });
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.call',
        [
            'PLACEMENT' => 'getStatus',
            'PARAMS' => (object)[]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | ID of the current call ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | Client's phone number ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the CRM entity associated with the call (`CONTACT`, `LEAD`, `COMPANY`) ||
|| **CRM_ENTITY_ID**
[`int`](../../../data-types.md) | ID of the CRM entity associated with the call ||
|| **CRM_ACTIVITY_ID**
[`int`](../../../data-types.md) | ID of the CRM activity related to the call ||
|| **CALL_DIRECTION**
[`string`](../../../data-types.md) | Direction of the call (`incoming`, `outgoing`, `incomingTransfer`, `callback`) ||
|| **CALL_LIST_MODE**
[`bool`](../../../data-types.md) | Indicator of operation in call list mode ||
|| **CRM_BINDINGS** | Array of bindings of the call to CRM entities ||
|#

## Continue Learning

- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)