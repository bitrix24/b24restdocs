# How to Change or Delete Phone Numbers and Emails

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create and modify contacts in CRM

Contact data in CRM can contain multiple phone numbers and email addresses. Sometimes it is necessary to update existing values or remove unnecessary ones.

Let's create a contact with multiple emails and phone numbers, and then modify this information. To do this, we will sequentially execute three methods:

1. [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md) — create a contact in CRM,

2. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve information about the created contact,

3. [crm.contact.update](../../../api-reference/crm/contacts/crm-contact-update.md) — update the email and phone data.

## Fields crm_multifield

The system stores phone numbers and emails as an array of objects [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield). Each object has the following fields:

```javascript
{
    ID: 123, // Identifier of the existing record. Needed for updating
    TYPE_ID: "PHONE", // Type of the multifield
    VALUE: "test@test.com", // Value
    VALUE_TYPE: "WORK" // Type of the value
}
```

- To delete a value from a multifield, pass the identifier `ID` and an empty value `VALUE`. Alternatively, specify the parameter `DELETE: 'Y'` instead of `VALUE`.

- To update a value in a multifield, pass the identifier and the new value.

## Example with Email

### 1. Adding a Contact with Two Emails

To create a contact in CRM, we will execute the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md). In the `fields` object, we will pass the fields:

- `NAME` — the name of the contact,

- `EMAIL` — an array of email addresses from `arNewEmail`.

{% note warning "" %}

Check which required fields are set for contacts in your Bitrix24. All required fields must be passed to the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

{% endnote %}

```javascript
// preparing addresses in crm_multifield format
let arNewEmail = [
 { VALUE: 'work_email@nomail.com', VALUE_TYPE: 'WORK' },
 { VALUE: 'home_email@nomail.com', VALUE_TYPE: 'HOME' }
];

// creating a new contact
BX24.callMethod(
	"crm.contact.add",
	{
		fields: {
			NAME: 'New Contact',
			EMAIL: arNewEmail
 		}
	}
);
```

As a result, we will receive the identifier of the new contact, for example, `25`.

```json
{
	"result": 25
}
```

### 2. Retrieving the Contact for Editing

To get information about the created contact, we use the method [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) with the identifier `ID` from the result of the previous request.

```javascript
let contactId = newContact.data().result; // saving the ID of the created contact in a variable
// retrieving information about the contact by ID
BX24.callMethod(
	"crm.contact.get",
	{
		ID: contactId
	}
);
```

As a result, we will receive a description of all fields of the new contact.

```json
{
    "result": {
        "ID": "25",
        "NAME": "New Contact",
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

- `ID` — the identifier of the contact,

- `FIELDS` — an array of fields to be changed. We will pass the `EMAIL` field in the array and the new address values: for the first address, we will specify a new email, and for the second — `DELETE: 'Y'` to remove it.

```javascript
// preparing an array with new email information
let arUpdateEmail = [
 { ID: contactData.EMAIL[0].ID, VALUE: 'new_work_email@example.com' }, // changing value for the first email
 { ID: contactData.EMAIL[1].ID, 'DELETE': 'Y' } // removing the second value
];

// updating the contact
BX24.callMethod(
	"crm.contact.update",
	{
		ID: contactId,
		FIELDS: {
			EMAIL: arUpdateEmail
		}
	}
);
```

Upon successful update, the method will return `true`.

```json
{
    "result": true,
}
```

### Full Code Example

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    let arNewEmail = [
        {
            'VALUE': 'work_email@nomail.com',
            'VALUE_TYPE': 'WORK'
        },
        {
            'VALUE': 'home_email@nomail.com',
            'VALUE_TYPE': 'HOME'
        }
    ];

    // Step 1: Create a contact
    BX24.callMethod(
        "crm.contact.add",
        {
            fields: {
                'NAME': 'New Contact',
                'EMAIL': arNewEmail
            }
        },
        function(newContact) {
            if (newContact.error()) {
                console.error('Error creating contact: ' + newContact.error_description());
            } else {
                let contactId = newContact.data().result;

                // Step 2: Retrieve contact data
                BX24.callMethod(
                    "crm.contact.get",
                    {
                        ID: contactId
                    },
                    function(newContactData) {

                        // Check for email presence
                        if (newContactData.data().result.EMAIL?.length >= 2) {
                            let contactData = newContactData.data().result;

                            // Step 3: Prepare email update
                            let arUpdateEmail = [
                                {
                                    'ID': contactData.EMAIL[0].ID,
                                    'VALUE': 'new_work_email@example.com'
                                },
                                {
                                    'ID': contactData.EMAIL[1].ID,
                                    'DELETE': 'Y'
                                }
                            ];

                            // Updating contact
                            BX24.callMethod(
                                "crm.contact.update",
                                {
                                    ID: contactId,
                                    FIELDS: {
                                        'EMAIL': arUpdateEmail
                                    }
                                },
                                function(resultContactChange) {
                                    if (resultContactChange.error()) {
                                        console.error('Error updating contact:', resultContactChange.error());
                                    } else {
                                        console.log('Contact successfully updated');
                                    }
                                }
                            );
                        } else {
                            console.warn('Not enough emails found for update.');
                        }
                    }
                );
            }
        }
    );
    ```

- PHP

    ```php
    <?php
    require_once('crest.php');

    // Preparing an array of emails in multifield format
    $newEmail = [
        ['VALUE' => 'work_email@nomail.com', 'VALUE_TYPE' => 'WORK'],
        ['VALUE' => 'home_email@nomail.com', 'VALUE_TYPE' => 'HOME']
    ];

    // Creating a contact
    $newContact = CRest::call('crm.contact.add', [
        'fields' => [
            'NAME' => 'New Contact',
            'EMAIL' => $newEmail
        ]
    ]);

    if (!empty($newContact['result'])) {
        $contactId = $newContact['result'];

        // Step 2: Retrieve contact data
        $contactData = CRest::call('crm.contact.get', ['ID' => $contactId]);

        if (!empty($contactData['result']['EMAIL'][0]) && !empty($contactData['result']['EMAIL'][1])) {
            // Step 3: Prepare email update
            $updateEmail = [
                ['ID' => $contactData['result']['EMAIL'][0]['ID'], 'VALUE' => 'new_work_email@example.com'],
                ['ID' => $contactData['result']['EMAIL'][1]['ID'], 'DELETE' => 'Y'] // Removing second email
            ];

            // Updating contact
            $changeResult = CRest::call('crm.contact.update', [
                'ID' => $contactId,
                'FIELDS' => ['EMAIL' => $updateEmail]
            ]);

            if (!empty($changeResult['error'])) {
                echo 'Error updating contact: ' . $changeResult['error_description'];
            } else {
                echo 'Contact successfully updated.';
            }
        } else {
            echo 'No emails found for update.';
        }
    } else {
        echo 'Error creating contact: ' . $newContact['error_description'];
    }
    ?>
    ```

{% endlist %}

## Example with Phone Numbers

Similarly, you can update the list of phone numbers for the contact `PHONE`.

### 1. Adding a Contact with Two Phone Numbers

To create a contact in CRM, we will execute the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md). In the `fields` object, we will pass the fields:

- `NAME` — the name of the contact,

- `PHONE` — an array of phone numbers from `arNewPhone`.

{% note warning "" %}

Check which required fields are set for contacts in your Bitrix24. All required fields must be passed to the method [crm.contact.add](../../../api-reference/crm/contacts/crm-contact-add.md).

{% endnote %}

```javascript
// preparing phones in crm_multifield format
let arNewPhone = [
	{ VALUE: '89991234567', VALUE_TYPE: 'WORK' },
	{ VALUE: '89997654321', VALUE_TYPE: 'HOME' }
];
// creating a new contact
BX24.callMethod(
	"crm.contact.add",
	{
		fields: {
			NAME: 'New Contact',
			PHONE: arNewPhone
		}
	}
);
```

As a result, we will receive the identifier of the new contact, for example, `25`.

```json
{
	"result": 25
}
```

### 2. Retrieving the Contact for Editing

To get information about the created contact, we use the method [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) with the identifier `ID` obtained from the previous request.

```javascript
let contactId = newContact.data().result; // saving the ID of the created contact in a variable
// retrieving information about the contact by ID
BX24.callMethod(
	"crm.contact.get",
	{
		ID: contactId
	}
);
```

As a result, we will receive a description of all fields of the new contact.

```json
{
    "result": {
        "ID": "25",
        "NAME": "New Contact",
		..., // other fields
        "PHONE": [
            {
                "ID": "1971",
                "VALUE_TYPE": "WORK",
                "VALUE": "89991234567",
                "TYPE_ID": "PHONE"
            },
            {
                "ID": "1973",
                "VALUE_TYPE": "HOME",
                "VALUE": "89997654321",
                "TYPE_ID": "PHONE"
            }
        ]
    }
}
```

### 3. Updating the Phone List

To change the phone list, we will execute the method [crm.contact.update](../../../api-reference/crm/contacts/crm-contact-update.md).

- `ID` — the identifier of the contact,

- `FIELDS` — an array of fields to be changed. We will pass the `PHONE` field in the array and the new phone values: for the first phone, we will specify a new value, and for the second — an empty value to remove it.

```javascript
// preparing an array with new phone information
let arUpdatePhone = [
	{ ID: contactData.PHONE[0].ID, VALUE: '81119876541' },
	{ ID: contactData.PHONE[1].ID, VALUE: '' }
 ];
// updating the contact
BX24.callMethod(
	"crm.contact.update",
	{
		ID: contactId,
		FIELDS: {
			PHONE: arUpdatePhone
		}
	}
);
```

Upon successful update, the method will return `true`.

```json
{
    "result": true,
}
```

### Full Code Example

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    let arNewPhone = [
        { VALUE: '89991234567', VALUE_TYPE: 'WORK' },
        { VALUE: '89997654321', VALUE_TYPE: 'HOME' }
    ];

    // Step 1: Create a contact
    BX24.callMethod(
        "crm.contact.add",
        {
            fields: {
                NAME: 'New Contact',
                PHONE: arNewPhone
            }
        },
        function(newContact) {
            if (newContact.error()) {
                console.error('Error creating contact: ' + newContact.error_description());
            } else {
                let contactId = newContact.data().result;

                // Step 2: Retrieve contact data
                BX24.callMethod(
                    "crm.contact.get",
                    {
                        ID: contactId
                    },
                    function(contactData) {

                        // Check for phone presence
                        if (contactData.data().result.PHONE?.length >= 2) {
                            let phoneData = contactData.data().result;

                            // Step 3: Prepare phone update
                            let arUpdatePhone = [
                                {
                                    ID: phoneData.PHONE[0].ID,
                                    VALUE: '81119876541'
                                },
                                {
                                    ID: phoneData.PHONE[1].ID,
                                    VALUE: ''
                                }
                            ];

                            // Updating contact
                            BX24.callMethod(
                                "crm.contact.update",
                                {
                                    ID: contactId,
                                    FIELDS: {
                                        PHONE: arUpdatePhone
                                    }
                                },
                                function(resultContactChange) {
                                    if (resultContactChange.error()) {
                                        console.error('Error updating contact:', resultContactChange.error());
                                    } else {
                                        console.log('Contact successfully updated');
                                    }
                                }
                            );
                        } else {
                            console.warn('Not enough phones found for update.');
                        }
                    }
                );
            }
        }
    );
    ```

- PHP

    ```php
    <?php
    require_once('crest.php');

    // Preparing an array of phones in multifield format
    $newPhone = [
        ['VALUE' => '89991234567', 'VALUE_TYPE' => 'WORK'],
        ['VALUE' => '89997654321', 'VALUE_TYPE' => 'HOME']
    ];

    // Creating a contact
    $newContact = CRest::call('crm.contact.add', [
        'fields' => [
            'NAME' => 'New Contact',
            'PHONE' => $newPhone
        ]
    ]);

    if (!empty($newContact['result'])) {
        $contactId = $newContact['result'];

        // Step 2: Retrieve contact data
        $contactData = CRest::call('crm.contact.get', ['ID' => $contactId]);

        if (!empty($contactData['result']['PHONE'][0]) && !empty($contactData['result']['PHONE'][1])) {
            // Step 3: Prepare phone update
            $updatePhone = [
                ['ID' => $contactData['result']['PHONE'][0]['ID'], 'VALUE' => '81119876541'],
                ['ID' => $contactData['result']['PHONE'][1]['ID'], 'VALUE' => ''] // Removing second phone
            ];

            // Updating contact
            $changeResult = CRest::call('crm.contact.update', [
                'ID' => $contactId,
                'FIELDS' => ['PHONE' => $updatePhone]
            ]);

            if (!empty($changeResult['error'])) {
                echo 'Error updating contact: ' . $changeResult['error_description'];
            } else {
                echo 'Contact successfully updated.';
            }
        } else {
            echo 'No phones found for update.';
        }
    } else {
        echo 'Error creating contact: ' . $newContact['error_description'];
    }
    ?>
    ```

{% endlist %}