# Delete Custom Contact Field crm.contact.userfield.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.contact.userfield.delete` removes a custom contact field.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field associated with the contact.

The identifier can be obtained using the methods [`crm.contact.userfield.add`](./crm-contact-userfield-add.md) or [`crm.contact.userfield.list`](./crm-contact-userfield-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Delete the custom field with `id = 432`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":432}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":432,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.userfield.delete',
        {
            id: 432,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.userfield.delete',
        [
            'id' => 432
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php       
    try {
        $userfieldId = 123; // Replace with the actual userfield ID you want to delete
        $result = $serviceBuilder
            ->getCRMScope()
            ->contactUserfield()
            ->delete($userfieldId);
        if ($result->isSuccess()) {
            print("Deleted item successfully.");
        } else {
            print("Failed to delete item.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1724316817.995457,
        "finish": 1724316818.640754,
        "duration": 0.6452970504760742,
        "processing": 0.3215677738189697,
        "date_start": "2024-08-22T10:53:37+02:00",
        "date_finish": "2024-08-22T10:53:38+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `ID is not defined or invalid` | The provided `id` is either less than or equal to zero or not provided at all ||
|| `-` | `Access denied` | Occurs when:
- the user does not have administrative rights
- the user attempts to delete a custom field not associated with contacts ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The custom field with the provided `id` does not exist ||
|| `-` | Error deleting `FIELD_NAME` for object `ENTITY_ID` | Unknown error during deletion ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-update.md)
- [{#T}](./crm-contact-userfield-get.md)
- [{#T}](./crm-contact-userfield-list.md)