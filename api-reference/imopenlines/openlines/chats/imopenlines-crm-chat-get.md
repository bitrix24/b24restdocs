# Get Chat for CRM Object imopenlines.crm.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- no response in case of an error

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves chats for a CRM object.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CRM_ENTITY_TYPE*** 
[`string`](../../../data-types.md) | Type of CRM object: 
- `lead` — lead
- `deal` — deal
- `company` — company
- `contact` — contact
 ||
|| **CRM_ENTITY*** 
[`integer`](../../../data-types.md) | Identifier of the CRM object ||
|| **ACTIVE_ONLY**
[`boolean`](../../../data-types.md) | Return only active chats.

Possible values:
- `Y` — will return only active chats
- `N` — will return all chats
 
Default — `Y` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CRM_ENTITY_TYPE":"deal","CRM_ENTITY":288,"ACTIVE_ONLY":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.chat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CRM_ENTITY_TYPE":"deal","CRM_ENTITY":288,"ACTIVE_ONLY":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.crm.chat.get
    ```

- JS

    ```js
    BX24.callMethod(
        'imopenlines.crm.chat.get',
        {
            CRM_ENTITY_TYPE: 'deal',
            CRM_ENTITY: 288,
            ACTIVE_ONLY: 'N'
        },
        function(result) {
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

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.crm.chat.get',
        [
            'CRM_ENTITY_TYPE' => 'deal',
            'CRM_ENTITY' => 288,
            'ACTIVE_ONLY' => 'N'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "CHAT_ID": "9852",
            "CONNECTOR_ID": "livechat",
            "CONNECTOR_TITLE": "Live Chat"
        }
    ]
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Array of objects. Each object contains chat description ||
|| **CHAT_ID**
[`string`](../../data-types.md) | Identifier of the chat ||
|| **CONNECTOR_ID**
[`string`](../../data-types.md) | Identifier of the connector ||
|| **CONNECTOR_TITLE**
[`string`](../../data-types.md) | Title of the connector ||
|#

## Error Handling

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access ||
|| **ERROR_ARGUMENT** | One of the arguments is missing or incorrect ||
|#