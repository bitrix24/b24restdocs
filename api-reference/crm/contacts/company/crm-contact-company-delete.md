# Delete Company from Specified Contact crm.contact.company.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.company.delete` removes a company from the specified contact.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the contact.

The identifier can be obtained using the methods [crm.contact.list](../crm-contact-list.md) or [crm.contact.add](../crm-contact-add.md) ||
|| **fields***
[`object`][1] | An object containing information about which company needs to be removed from the bindings.

Contains a single key `COMPANY_ID` ||
|| **fields.COMPANY_ID***
[`integer`][1] | Identifier of the company to be removed from the bindings ||
|#

{% note info "Remove Primary Binding" %}

If the primary binding is removed, the first available binding will become the new primary binding.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of removing the contact-company link, where:
- contact identifier — `54`
- company identifier — `32`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":54,"fields":{"COMPANY_ID":32}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.company.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":54,"fields":{"COMPANY_ID":32},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.company.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.company.delete',
        {
            id: 54,
            fields: {
                COMPANY_ID: 32,
            },
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
        'crm.contact.company.delete',
        [
            'id' => 54,
            'fields' => [
                'COMPANY_ID' => 32,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1724072653.79827,
        "finish": 1724072717.749956,
        "duration": 63.95168590545654,
        "processing": 63.63148093223572,
        "date_start": "2024-08-19T15:04:13+02:00",
        "date_finish": "2024-08-19T15:05:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response. Contains:
- `true` — on success
- `false` — on failure (most likely the company you are trying to remove is not linked to the contact)
||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The parameter 'ownerEntityID' is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `The parameter 'ownerEntityID' is invalid or not defined` | The provided `id` is less than 0 ||
|| `-`     | `The parameter 'fields' must be array` | The `fields` parameter is not an object ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to edit contacts ||
|| `-`     | `Not found` | Contact with the provided `id` was not found ||
|| `-`     | `The parameter 'fields' is not valid` | May occur due to several reasons:
- if the required parameter `fields.COMPANY_ID` is not provided
- if the provided parameter `fields.COMPANY_ID` is less than or equal to 0 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-company-add.md)
- [{#T}](./crm-contact-company-fields.md)
- [{#T}](./crm-contact-company-items-get.md)
- [{#T}](./crm-contact-company-items-set.md)
- [{#T}](./crm-contact-company-items-delete.md)

[1]: ../../../data-types.md