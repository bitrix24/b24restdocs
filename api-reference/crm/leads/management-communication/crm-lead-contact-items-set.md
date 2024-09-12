# Attach a List of Contacts to a Lead crm.lead.contact.items.set

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method attaches a list of contacts to the specified lead.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the lead. The lead identifier can be obtained using the [get lead list method](../crm-lead-list.md) ||
|| **items***
[`object`](../../../data-types.md) | A set of contacts represented as an array of objects with the following fields:

- **CONTACT_ID^*^** — identifier of the contact
- **SORT** — sorting index
- **IS_PRIMARY** — primary contact flag 
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"items":[{"CONTACT_ID":1010,"SORT":10,"IS_PRIMARY":"Y"},{"CONTACT_ID":1020,"SORT":20,"IS_PRIMARY":"N"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.contact.items.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"items":[{"CONTACT_ID":1010,"SORT":10,"IS_PRIMARY":"Y"},{"CONTACT_ID":1020,"SORT":20,"IS_PRIMARY":"N"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.contact.items.set
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.lead.contact.items.set",
        {
            id: 1,
            items: [
                {
                    "CONTACT_ID": 1010,
                    "SORT": 10,
                    "IS_PRIMARY": "Y"
                },
                {
                    "CONTACT_ID": 1020,
                    "SORT": 20,
                    "IS_PRIMARY": "N"
                }
            ]
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.contact.items.set',
        [
            'id' => 1,
            'items' => [
                [
                    'CONTACT_ID' => 1010,
                    'SORT' => 10,
                    'IS_PRIMARY' => 'Y'
                ],
                [
                    'CONTACT_ID' => 1020,
                    'SORT' => 20,
                    'IS_PRIMARY' => 'N'
                ]
            ]
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
    "result": true,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+03:00",
        "date_finish": "2024-05-03T17:19:01+03:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the operation ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `NOT_FOUND` | Element not found ||
|| ` ` | Required fields not provided ||
|| ` ` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-lead-contact-add.md)
- [{#T}](./crm-lead-contact-delete.md)
- [{#T}](./crm-lead-contact-items-get.md)
- [{#T}](./crm-lead-contact-items-delete.md)
- [{#T}](./crm-lead-contact-fields.md)