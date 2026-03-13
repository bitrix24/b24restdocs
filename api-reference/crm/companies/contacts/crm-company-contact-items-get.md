# Retrieve a Set of Contacts Associated with a Specified Company crm.company.contact.items.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for companies

The method `crm.company.contact.items.get` returns a set of contacts associated with the specified company.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the company.

The identifier can be obtained using the methods [crm.company.list](../crm-company-list.md) or [crm.company.add](../crm-company-add.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.contact.items.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.contact.items.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.company.contact.items.get',
            {
                id: 32,
            }
        );
        
        const result = response.getData().result;
        result.error()
            ? console.error(result.error())
            : console.info(result)
        ;
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.contact.items.get',
                [
                    'id' => 32,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting company contact items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.contact.items.get',
        {
            id: 32,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.contact.items.get',
        [
            'id' => 32
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
    "result": [
        {
            "CONTACT_ID": 7,
            "SORT": 100,
            "ROLE_ID": 0,
            "IS_PRIMARY": "Y"
        },
        {
            "CONTACT_ID": 8,
            "SORT": 110,
            "ROLE_ID": 0,
            "IS_PRIMARY": "N"
        },
        {
            "CONTACT_ID": 9,
            "SORT": 120,
            "ROLE_ID": 0,
            "IS_PRIMARY": "N"
        }
    ],
    "time": {
        "start": 1724078791.470108,
        "finish": 1724078791.969407,
        "duration": 0.4992990493774414,
        "processing": 0.19150400161743164,
        "date_start": "2024-08-19T16:46:31+02:00",
        "date_finish": "2024-08-19T16:46:31+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`company_contact_binding[]`](#company_contact_binding) | Root element of the response. Contains an array with information about the contacts associated with the company ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Parameter company_contact_binding {#company_contact_binding}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID**
[`integer`](../../../data-types.md) | Identifier of the contact ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index ||
|| **ROLE_ID**
[`integer`](../../../data-types.md) | Role identifier, a system field ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md#char) | Indicates whether the binding is primary. Possible values:
- `Y` — yes
- `N` — no ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The parameter ownerEntityID is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `The parameter ownerEntityID is invalid or not defined` | The provided `id` is less than or equal to 0 or not provided at all ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to read companies ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-contact-add.md)
- [{#T}](./crm-company-contact-delete.md)
- [{#T}](./crm-company-contact-fields.md)
- [{#T}](./crm-company-contact-items-set.md)
- [{#T}](./crm-company-contact-items-delete.md)