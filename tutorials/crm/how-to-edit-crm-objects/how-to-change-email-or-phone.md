# How to Change or Delete Phone Numbers and Emails

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create and modify contacts in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Contact data in CRM can contain multiple phone numbers and email addresses. Sometimes, it is necessary to update existing values or remove unnecessary ones.

Let's create a contact with multiple emails and phone numbers, and then modify this information. To do this, we will sequentially execute three methods:

1. [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) — create a contact in CRM,

2. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve information about the created contact,

3. [crm.contact.update](../../../api-reference/crm/contacts/crm-contact-update.md) — update the email and phone data.

## Fields crm_multifield

The system stores phone numbers and emails as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield). Each object has the following fields:

```javascript
{
    ID: 123, // Existing record identifier. Required for update
    TYPE_ID: "PHONE" // Multiple field type
    VALUE: "test@test.com", // Value
    VALUE_TYPE: "WORK" // Value type
}
```

- To delete a value from a multi-field, pass the `ID` identifier and an empty value `VALUE`. Another option — specify parameter `DELETE: 'Y'` instead of `VALUE`.

- To update a multi-field value, pass the identifier and the new value.

## Example with Email

### 1. Adding a Contact with Two Emails

To create a contact in CRM, we will execute the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md). In the `fields` object, we will pass the fields:

- `NAME` — the contact name,

- `EMAIL` — an array of email addresses from `arNewEmail`.

{% note warning "" %}

Check which required fields are set for contacts in your Bitrix24. All required fields must be passed to the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))
    ```

{% endlist %}

As a result, we will receive the identifier of the new contact, for example, `25`.

```json
{
	"result": 25
}
```

### 2. Retrieving the Contact for Editing

To retrieve information about the created contact, use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the `ID` identifier from the previous request's result.

{% list tabs %}

- JS

    ```javascript
    // get contact information by ID
    const response = await $b24.actions.v2.call.make({
        method: 'crm.contact.get',
        params: { ID: contactId },
        requestId: 'contact-get'
    })
    const contactData = response.getData().result
    ```

- PHP

    ```php
    // get contact information by ID
    $contactData = $sb->getCRMScope()->contact()->get($contactId)->contact();
    ```

- Python

    ```python
    # get contact information by ID
    contact_data = client.crm.contact.get(bitrix_id=contact_id).result
    ```

{% endlist %}

As a result, we will receive a description of all fields for the new contact.

```json
{
    "result": {
        "ID": "25",
        "NAME": "New contact",
		..., // other fields
        "EMAIL": [
            {
                "ID": "1967",
                "VALUE_TYPE": "WORK",
                "VALUE": "work_email@nomail.com",
                "TYPE_ID": "EMAIL"
            },
            {
                "ID": "1969",
                "VALUE_TYPE": "HOME",
                "VALUE": "home_email@nomail.com",
                "TYPE_ID": "EMAIL"
            }
        ]
    }
}
```

### 3. Updating the Email List

To change the email list, we will execute the method [crm.contact.update](../../../api-reference/crm/contacts/crm-contact-update.md).

- `ID` — the contact identifier,

- `FIELDS` — an array of fields to be changed. We will pass the `EMAIL` field in the array along with the new address values: for the first address, we will specify a new email, and for the second, we will specify `DELETE: 'Y'` to delete it.

{% list tabs %}

- JS

    ```javascript
    // prepare an array with new email information
    const arUpdateEmail = [
        { ID: contactData.EMAIL[0].ID, VALUE: 'new_work_email@example.com' }, // change value for the first email
        { ID: contactData.EMAIL[1].ID, DELETE: 'Y' } // delete the second value
    ]

    // update contact
    await $b24.actions.v2.call.make({
        method: 'crm.contact.update',
        params: { ID: contactId, FIELDS: { EMAIL: arUpdateEmail } },
        requestId: 'contact-update'
    })
    ```

- PHP

    ```php
    // prepare an array with new email information
    $arUpdateEmail = [
        ['ID' => $contactData->EMAIL[0]->ID, 'VALUE' => 'new_work_email@example.com'], // change value for the first email
        ['ID' => $contactData->EMAIL[1]->ID, 'DELETE' => 'Y'], // delete the second value
    ];

    // update contact
    $sb->getCRMScope()->contact()->update($contactId, ['EMAIL' => $arUpdateEmail]);
    ```

- Python

    ```python
    # prepare an array with new email information
    ar_update_email = [
        {"ID": contact_data["EMAIL"][0]["ID"], "VALUE": "new_work_email@example.com"},  # change value for the first email
        {"ID": contact_data["EMAIL"][1]["ID"], "DELETE": "Y"},  # delete the second value
    ]

    # update contact
    client.crm.contact.update(bitrix_id=contact_id, fields={"EMAIL": ar_update_email})
    ```

{% endlist %}

Upon a successful update, the method will return `true`.

```json
{
    "result": true,
}
```

### Full Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const arNewEmail = [
        { VALUE: 'work_email@nomail.com', VALUE_TYPE: 'WORK' },
        { VALUE: 'home_email@nomail.com', VALUE_TYPE: 'HOME' }
    ]

    // Step 1: create contact
    const newContact = await $b24.actions.v2.call.make({
        method: 'crm.contact.add',
        params: { fields: { NAME: 'New contact', EMAIL: arNewEmail } },
        requestId: 'contact-add'
    })
    if (!newContact.isSuccess) {
        console.error('Error creating contact: ' + newContact.getErrorMessages().join('; '))
    } else {
        const contactId = newContact.getData().result

        // Step 2: get contact data
        const contactResponse = await $b24.actions.v2.call.make({
            method: 'crm.contact.get',
            params: { ID: contactId },
            requestId: 'contact-get'
        })
        const contactData = contactResponse.getData().result

        // Check if email exists
        if ((contactData.EMAIL?.length ?? 0) >= 2) {
            // Step 3: form email update
            const arUpdateEmail = [
                { ID: contactData.EMAIL[0].ID, VALUE: 'new_work_email@example.com' },
                { ID: contactData.EMAIL[1].ID, DELETE: 'Y' }
            ]

            // update contact
            const resultContactChange = await $b24.actions.v2.call.make({
                method: 'crm.contact.update',
                params: { ID: contactId, FIELDS: { EMAIL: arUpdateEmail } },
                requestId: 'contact-update'
            })
            if (!resultContactChange.isSuccess) {
                console.error('Error updating contact: ' + resultContactChange.getErrorMessages().join('; '))
            } else {
                console.log('Contact successfully updated')
            }
        } else {
            console.warn('Not enough emails found to update.')
        }
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');
    $contact = $sb->getCRMScope()->contact();

    // Form an email array in multifield format
    $newEmail = [
        ['VALUE' => 'work_email@nomail.com', 'VALUE_TYPE' => 'WORK'],
        ['VALUE' => 'home_email@nomail.com', 'VALUE_TYPE' => 'HOME'],
    ];

    try {
        // Step 1: create contact
        $contactId = $contact->add([
            'NAME' => 'New contact',
            'EMAIL' => $newEmail,
        ])->getId();

        // Step 2: get contact data
        $contactData = $contact->get($contactId)->contact();

        if (count($contactData->EMAIL) >= 2) {
            // Step 3: form email update
            $updateEmail = [
                ['ID' => $contactData->EMAIL[0]->ID, 'VALUE' => 'new_work_email@example.com'],
                ['ID' => $contactData->EMAIL[1]->ID, 'DELETE' => 'Y'], // delete the second email
            ];

            // update contact
            $contact->update($contactId, ['EMAIL' => $updateEmail]);
            echo 'Contact successfully updated.';
        } else {
            echo 'No emails found to update.';
        }
    } catch (\Throwable $e) {
        echo 'Error working with contact: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="USER_ID/TOKEN",
        )
    )

    try:
        new_contact_id = int(
            client.crm.contact.add(
                fields={
                    "NAME": "New contact",
                    "EMAIL": [
                        {"VALUE": "work_email@nomail.com", "VALUE_TYPE": "WORK"},
                        {"VALUE": "home_email@nomail.com", "VALUE_TYPE": "HOME"},
                    ],
                }
            ).response.result
        )
    except BitrixAPIError as error:
        print(f"Error creating contact: {error}")
    else:
        try:
            contact_data = client.crm.contact.get(
                bitrix_id=new_contact_id,
            ).response.result
        except BitrixAPIError as error:
            print(f"Error getting contact: {error}")
        else:
            if len(contact_data.get("EMAIL", [])) >= 2:
                update_email = [
                    {
                        "ID": contact_data["EMAIL"][0]["ID"],
                        "VALUE": "new_work_email@example.com",
                    },
                    {
                        "ID": contact_data["EMAIL"][1]["ID"],
                        "DELETE": "Y",
                    },
                ]

                try:
                    change_result = client.crm.contact.update(
                        bitrix_id=new_contact_id,
                        fields={"EMAIL": update_email},
                    ).response.result
                except BitrixAPIError as error:
                    print(f"Error updating contact: {error}")
                else:
                    if change_result:
                        print("Contact successfully updated.")
            else:
                print("No emails found to update.")
    ```

{% endlist %}

## Phone Number Example

Similarly, you can update the list of phone numbers for the contact `PHONE`.

### 1. Adding a Contact with Two Phone Numbers

To create a contact in the CRM, call the [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) method. In the `fields` object, pass the following fields:

- `NAME` — the contact name,

- `PHONE` — an array of phone numbers from `arNewPhone`.

{% note warning "" %}

Check which required fields are set for contacts in your Bitrix24. All required fields must be passed to the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))
    ```

{% endlist %}

As a result, you will receive the identifier of the new contact, for example, `25`.

```json
{
	"result": 25
}
```

### 2. Retrieving a Contact for Editing

To retrieve information about the created contact, use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the `ID` identifier obtained in the previous request.

{% list tabs %}

- JS

    ```javascript
    // get contact information by ID
    const response = await $b24.actions.v2.call.make({
        method: 'crm.contact.get',
        params: { ID: contactId },
        requestId: 'contact-get'
    })
    const contactData = response.getData().result
    ```

- PHP

    ```php
    // get contact information by ID
    $contactData = $sb->getCRMScope()->contact()->get($contactId)->contact();
    ```

- Python

    ```python
    # get contact information by ID
    contact_data = client.crm.contact.get(bitrix_id=contact_id).result
    ```

{% endlist %}

As a result, you will receive a description of all fields for the new contact.

```json
{
    "result": {
        "ID": "25",
        "NAME": "New contact",
		..., // other fields
        "PHONE": [
            {
                "ID": "1971",
                "VALUE_TYPE": "WORK",
                "VALUE": "499991234567",
                "TYPE_ID": "PHONE"
            },
            {
                "ID": "1973",
                "VALUE_TYPE": "HOME",
                "VALUE": "499997654321",
                "TYPE_ID": "PHONE"
            }
        ]
    }
}
```

### 3. Updating the Phone Number List

To change the list of phone numbers, call the [crm.contact.update](../../../api-reference/crm/contacts/crm-contact-update.md) method.

- `ID` — the contact identifier,

- `FIELDS` — an array of fields to be changed. We will pass the `PHONE` field in the array along with the new phone values: we will specify a new value for the first phone and an empty value for the second to delete it.

{% list tabs %}

- JS

    ```javascript
    // prepare an array with new phone information
    const arUpdatePhone = [
        { ID: contactData.PHONE[0].ID, VALUE: '81119876541' }, // change value for the first phone
        { ID: contactData.PHONE[1].ID, VALUE: '' } // empty value deletes the second phone
    ]

    // update contact
    await $b24.actions.v2.call.make({
        method: 'crm.contact.update',
        params: { ID: contactId, FIELDS: { PHONE: arUpdatePhone } },
        requestId: 'contact-update'
    })
    ```

- PHP

    ```php
    // prepare an array with new phone information
    $arUpdatePhone = [
        ['ID' => $contactData->PHONE[0]->ID, 'VALUE' => '81119876541'], // change value for the first phone
        ['ID' => $contactData->PHONE[1]->ID, 'VALUE' => ''], // empty value deletes the second phone
    ];

    // update contact
    $sb->getCRMScope()->contact()->update($contactId, ['PHONE' => $arUpdatePhone]);
    ```

- Python

    ```python
    # prepare an array with new phone information
    ar_update_phone = [
        {"ID": contact_data["PHONE"][0]["ID"], "VALUE": "81119876541"},  # change value for the first phone
        {"ID": contact_data["PHONE"][1]["ID"], "VALUE": ""},  # empty value deletes the second phone
    ]

    # update contact
    client.crm.contact.update(bitrix_id=contact_id, fields={"PHONE": ar_update_phone})
    ```

{% endlist %}

Upon a successful update, the method will return `true`.

```json
{
    "result": true,
}
```

### Full Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const arNewPhone = [
        { VALUE: '499991234567', VALUE_TYPE: 'WORK' },
        { VALUE: '499997654321', VALUE_TYPE: 'HOME' }
    ]

    // Step 1: create contact
    const newContact = await $b24.actions.v2.call.make({
        method: 'crm.contact.add',
        params: { fields: { NAME: 'New contact', PHONE: arNewPhone } },
        requestId: 'contact-add'
    })
    if (!newContact.isSuccess) {
        console.error('Error creating contact: ' + newContact.getErrorMessages().join('; '))
    } else {
        const contactId = newContact.getData().result

        // Step 2: get contact data
        const contactResponse = await $b24.actions.v2.call.make({
            method: 'crm.contact.get',
            params: { ID: contactId },
            requestId: 'contact-get'
        })
        const phoneData = contactResponse.getData().result

        // Check if phones exist
        if ((phoneData.PHONE?.length ?? 0) >= 2) {
            // Step 3: form phone update
            const arUpdatePhone = [
                { ID: phoneData.PHONE[0].ID, VALUE: '81119876541' },
                { ID: phoneData.PHONE[1].ID, VALUE: '' }
            ]

            // update contact
            const resultContactChange = await $b24.actions.v2.call.make({
                method: 'crm.contact.update',
                params: { ID: contactId, FIELDS: { PHONE: arUpdatePhone } },
                requestId: 'contact-update'
            })
            if (!resultContactChange.isSuccess) {
                console.error('Error updating contact: ' + resultContactChange.getErrorMessages().join('; '))
            } else {
                console.log('Contact successfully updated')
            }
        } else {
            console.warn('Not enough phones found.')
        }
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');
    $contact = $sb->getCRMScope()->contact();

    // Form a phone array in multifield format
    $newPhone = [
        ['VALUE' => '499991234567', 'VALUE_TYPE' => 'WORK'],
        ['VALUE' => '499997654321', 'VALUE_TYPE' => 'HOME'],
    ];

    try {
        // Step 1: create contact
        $contactId = $contact->add([
            'NAME' => 'New contact',
            'PHONE' => $newPhone,
        ])->getId();

        // Step 2: get contact data
        $contactData = $contact->get($contactId)->contact();

        if (count($contactData->PHONE) >= 2) {
            // Step 3: form phone update
            $updatePhone = [
                ['ID' => $contactData->PHONE[0]->ID, 'VALUE' => '81119876541'],
                ['ID' => $contactData->PHONE[1]->ID, 'VALUE' => ''], // empty value deletes the second phone
            ];

            // update contact
            $contact->update($contactId, ['PHONE' => $updatePhone]);
            echo 'Contact successfully updated.';
        } else {
            echo 'No phones found to update.';
        }
    } catch (\Throwable $e) {
        echo 'Error working with contact: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="USER_ID/TOKEN",
        )
    )

    try:
        contact_id = int(
            client.crm.contact.add(
                fields={
                    "NAME": "New contact",
                    "PHONE": [
                        {"VALUE": "499991234567", "VALUE_TYPE": "WORK"},
                        {"VALUE": "499997654321", "VALUE_TYPE": "HOME"},
                    ],
                }
            ).response.result
        )
    except BitrixAPIError as error:
        print(f"Error creating contact: {error}")
    else:
        try:
            contact = client.crm.contact.get(bitrix_id=contact_id).response.result
        except BitrixAPIError as error:
            print(f"Error getting contact: {error}")
        else:
            values = contact.get("PHONE") or []

            if len(values) >= 2:
                updated_values = [
                    {"ID": values[0]["ID"], "VALUE": "81119876541"},
                    {"ID": values[1]["ID"], "VALUE": ""},
                ]
                try:
                    change_result = client.crm.contact.update(
                        bitrix_id=contact_id,
                        fields={"PHONE": updated_values},
                    ).response.result
                except BitrixAPIError as error:
                    print(f"Error updating contact: {error}")
                else:
                    if change_result:
                        print("Contact successfully updated.")
            else:
                print("No phones found to update.")
    ```

{% endlist %}
