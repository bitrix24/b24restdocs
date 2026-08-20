# How to Change or Delete Phone Numbers and Emails

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to modify items of a CRM object
>
> - [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) — a user with permission to modify items of a CRM object
> - [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — a user with permission to create items of a CRM object
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read items of a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 retains phone numbers and emails not as separate contact fields but in the `fm` multifield — a set of entries of the [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) type. A single contact can have any number of such entries: a work email and a personal one, a mobile phone and a work phone.

Every entry has its own `id`, which Bitrix24 assigns when the entry is created. A particular number can be modified or deleted only by this identifier — the system does not look up an entry by the text of its value. That is why you first read the contact and retrieve the identifiers, and only then send the changes.

As a result of the scenario, the work email and the mobile phone of the contact change, the personal email is deleted, and the work phone remains as it was.

The scenario consists of three steps.

1. Create a contact with an email and phone numbers using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method
2. Read the contact and retrieve the `id` values of the multifield entries using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method
3. Change some values and delete others using the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method

If the contact already exists, skip step 1 and start with step 2.

## Before You Start

- The webhook is created on behalf of a user who has permission to modify contacts in CRM

- The `crm` scope is selected in the webhook permissions

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

- Pass the required custom fields of the contact yourself in step 1: whether the method takes them into account depends on the CRM setting "Check for required custom fields"

The scenario is shown for a contact — `entityTypeId`: `3`. Multifields work the same way for a contact, a company, and a lead: what to change for other objects is described in the [Key Considerations](#other-types) section.

## How the Key in the fm Field Determines the Operation {#fm-format}

In step 1, the `fm` field is an array: every element adds a new entry.

In step 3, the `fm` field is an object, and the key of an element determines the operation.

#|
|| **Key** | **What It Does** | **What to Pass in the Element** ||
|| `n0`, `n1`, `n2` ... | Adds a new entry | `typeId`, `valueType`, `value` ||
|| The numeric `id` of an entry | Changes the value of an existing entry | `typeId`, `valueType`, the new `value` ||
|| The numeric `id` of an entry | Deletes the entry | `typeId` and an empty `value` ||
|#

Every element carries three keys:

- `typeId` — the entry type: `PHONE`, `EMAIL`, `WEB`, `IM`, `LINK`

- `valueType` — the value subtype: for a phone — `WORK`, `MOBILE`, `FAX`, `HOME`, `PAGER`, `MAILING`, `OTHER`; for an email — `WORK`, `HOME`, `MAILING`, `OTHER`

- `value` — the value itself

{% note warning "" %}

`typeId` is required in every element of the `fm` object, including deletion. Without it, the method does not return an error but does not perform the operation either: the response is successful, while the value remains as it was.

{% endnote %}

The method does not touch entries that are absent from the `fm` object. That is why deletion is set as a separate operation with an empty `value` rather than by omitting the entry from the request.

## 1. Create a Contact with an Email and Phone Numbers {#add}

Use the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method with the following parameters:

- `entityTypeId` — the identifier of the [CRM object type](../../../api-reference/crm/data-types.md#object_type), a required parameter. Pass `3` — a contact

- `fields[name]` and `fields[lastName]` — the first name and the last name of the contact

- `fields[fm]` — an array of multifield entries. Pass two emails and two phone numbers

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const resultAdd = await $b24.actions.v2.call.make({
        method: 'crm.item.add',
        params: {
            entityTypeId: 3, // 3 — contact
            fields: {
                name: 'Klaus',
                lastName: 'Weber',
                fm: [
                    { typeId: 'EMAIL', valueType: 'WORK', value: 'work_email@nomail.com' },
                    { typeId: 'EMAIL', valueType: 'HOME', value: 'home_email@nomail.com' },
                    { typeId: 'PHONE', valueType: 'WORK', value: '+493012345678' },
                    { typeId: 'PHONE', valueType: 'MOBILE', value: '+493076543210' }
                ]
            }
        },
        requestId: 'item-add'
    });

    const contactId = resultAdd.getData().result.item.id;
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

    $resultAdd = $sb->getCRMScope()->item()->add(
        3, // 3 — contact
        [
            'name' => 'Klaus',
            'lastName' => 'Weber',
            'fm' => [
                [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'work_email@nomail.com' ],
                [ 'typeId' => 'EMAIL', 'valueType' => 'HOME', 'value' => 'home_email@nomail.com' ],
                [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+493012345678' ],
                [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+493076543210' ]
            ]
        ]
    );

    $contactId = $resultAdd->item()->id;
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
        )
    )

    item = client.crm.item.add(
        3,  # 3 — contact
        {
            "name": "Klaus",
            "lastName": "Weber",
            "fm": [
                {"typeId": "EMAIL", "valueType": "WORK", "value": "work_email@nomail.com"},
                {"typeId": "EMAIL", "valueType": "HOME", "value": "home_email@nomail.com"},
                {"typeId": "PHONE", "valueType": "WORK", "value": "+493012345678"},
                {"typeId": "PHONE", "valueType": "MOBILE", "value": "+493076543210"},
            ],
        },
    ).response.result["item"]

    contact_id = item["id"]
    ```

{% endlist %}

The method returns an `item` object with the full set of contact fields. The response is shortened to the fields that confirm the result. Retain the `id` of the contact — steps 2 and 3 need it. In the example, `id`: `2653`.

```json
{
    "result": {
        "item": {
            "id": 2653,
            "entityTypeId": 3,
            "name": "Klaus",
            "lastName": "Weber",
            "hasPhone": "Y",
            "hasEmail": "Y",
            "fm": [
                {
                    "id": 8553,
                    "valueType": "WORK",
                    "value": "work_email@nomail.com",
                    "typeId": "EMAIL"
                },
                {
                    "id": 8555,
                    "valueType": "HOME",
                    "value": "home_email@nomail.com",
                    "typeId": "EMAIL"
                },
                {
                    "id": 8557,
                    "valueType": "WORK",
                    "value": "+493012345678",
                    "typeId": "PHONE"
                },
                {
                    "id": 8559,
                    "valueType": "MOBILE",
                    "value": "+493076543210",
                    "typeId": "PHONE"
                }
            ]
        }
    }
}
```

## 2. Retrieve the Identifiers of the Multifield Entries {#get}

The `crm.item.add` method has already returned the `fm` array with the identifiers, so within a single script step 2 can be skipped. A separate call is needed when the contact was created earlier and you do not have the identifiers.

Use the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method with the following parameters:

- `entityTypeId` — `3` for a contact

- `id` — the identifier of the contact from [step 1](#add), `2653` in the example

{% list tabs %}

- JS

    ```javascript
    const resultGet = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: {
            entityTypeId: 3,
            id: contactId
        },
        requestId: 'item-get'
    });

    const multifields = resultGet.getData().result.item.fm;
    ```

- PHP

    ```php
    $multifields = $sb->getCRMScope()->item()->get(3, $contactId)->item()->fm;
    ```

- Python

    ```python
    multifields = client.crm.item.get(
        3,
        contact_id,
    ).response.result["item"]["fm"]
    ```

{% endlist %}

In the `fm` array, locate the entries you need by the `typeId` and `valueType` pair and retain their `id` values. In the example, the work email is `8553`, the personal email is `8555`, and the mobile phone is `8559`.

```json
{
    "result": {
        "item": {
            "id": 2653,
            "name": "Klaus",
            "lastName": "Weber",
            "fm": [
                {
                    "id": 8553,
                    "valueType": "WORK",
                    "value": "work_email@nomail.com",
                    "typeId": "EMAIL"
                },
                {
                    "id": 8555,
                    "valueType": "HOME",
                    "value": "home_email@nomail.com",
                    "typeId": "EMAIL"
                },
                {
                    "id": 8557,
                    "valueType": "WORK",
                    "value": "+493012345678",
                    "typeId": "PHONE"
                },
                {
                    "id": 8559,
                    "valueType": "MOBILE",
                    "value": "+493076543210",
                    "typeId": "PHONE"
                }
            ]
        }
    }
}
```

{% note warning "" %}

Do not hardcode the entry identifiers from the example into production code. Bitrix24 assigns them at the moment an entry is created, so every contact has its own — request them in step 2 and substitute them as variables, as shown in the [Code Example](#full-example) section.

{% endnote %}

## 3. Change and Delete Multifield Entries {#update}

Use the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method with the following parameters:

- `entityTypeId` — `3` for a contact

- `id` — the identifier of the contact from [step 1](#add), `2653` in the example

- `fields[fm]` — an object with the operations on the multifield entries. The key of every element is the `id` of an entry from [step 2](#get), and the content determines the operation, as described in the [How the Key in the fm Field Determines the Operation](#fm-format) section

Perform three operations in a single request:

- change the work email `8553` — pass a new `value`

- delete the personal email `8555` — pass an empty `value`

- change the mobile phone `8559` — pass a new `value`

The work phone `8557` is not mentioned in the request, so it remains as it was.

{% list tabs %}

- JS

    ```javascript
    const resultUpdate = await $b24.actions.v2.call.make({
        method: 'crm.item.update',
        params: {
            entityTypeId: 3,
            id: contactId,
            fields: {
                fm: {
                    // the key is the entry id from step 2
                    8553: { typeId: 'EMAIL', valueType: 'WORK', value: 'new_work_email@nomail.com' }, // change the work email
                    8555: { typeId: 'EMAIL', value: '' }, // an empty value deletes the personal email
                    8559: { typeId: 'PHONE', valueType: 'MOBILE', value: '+493055544433' } // change the mobile phone
                }
            }
        },
        requestId: 'item-update'
    });
    ```

- PHP

    ```php
    $resultUpdate = $sb->getCRMScope()->item()->update(
        3,
        $contactId,
        [
            'fm' => [
                // the key is the entry id from step 2
                8553 => [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'new_work_email@nomail.com' ], // change the work email
                8555 => [ 'typeId' => 'EMAIL', 'value' => '' ], // an empty value deletes the personal email
                8559 => [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+493055544433' ] // change the mobile phone
            ]
        ]
    );
    ```

- Python

    ```python
    result_update = client.crm.item.update(
        3,
        contact_id,
        {
            "fm": {
                # the key is the entry id from step 2
                "8553": {"typeId": "EMAIL", "valueType": "WORK", "value": "new_work_email@nomail.com"},  # change the work email
                "8555": {"typeId": "EMAIL", "value": ""},  # an empty value deletes the personal email
                "8559": {"typeId": "PHONE", "valueType": "MOBILE", "value": "+493055544433"},  # change the mobile phone
            },
        },
    ).response.result["item"]
    ```

{% endlist %}

The method returns the entire contact with the new set of entries, so a separate request to check the result is not required. The response is shortened.

```json
{
    "result": {
        "item": {
            "id": 2653,
            "name": "Klaus",
            "lastName": "Weber",
            "hasEmail": "Y",
            "hasPhone": "Y",
            "updatedTime": "2026-08-20T09:10:29+03:00",
            "fm": [
                {
                    "id": 8553,
                    "valueType": "WORK",
                    "value": "new_work_email@nomail.com",
                    "typeId": "EMAIL"
                },
                {
                    "id": 8557,
                    "valueType": "WORK",
                    "value": "+493012345678",
                    "typeId": "PHONE"
                },
                {
                    "id": 8559,
                    "valueType": "MOBILE",
                    "value": "+493055544433",
                    "typeId": "PHONE"
                }
            ]
        }
    }
}
```

## Verify the Result

Open the card of the contact "Klaus Weber" in CRM. The contact details block now holds the work email `new_work_email@nomail.com`, the work phone `+493012345678`, and the mobile phone `+493055544433`. The personal email `home_email@nomail.com` is gone. Bitrix24 displays phone numbers in its own format, so compare them digit by digit rather than by their spelling.

Over REST, the set of multifields is returned by the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method with the same parameters as in step 2.

{% list tabs %}

- JS

    ```javascript
    const checkResult = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: { entityTypeId: 3, id: contactId },
        requestId: 'item-check'
    });

    console.dir(checkResult.getData().result.item.fm);
    ```

- PHP

    ```php
    print_r($sb->getCRMScope()->item()->get(3, $contactId)->item()->fm);
    ```

- Python

    ```python
    print(client.crm.item.get(3, contact_id).response.result["item"]["fm"])
    ```

{% endlist %}

The scenario is complete if the `fm` array holds three entries: `8553` has a new email address, `8559` has a new phone number, entry `8555` is gone, and `8557` is unchanged.

```json
{
    "result": {
        "item": {
            "id": 2653,
            "fm": [
                {
                    "id": 8553,
                    "valueType": "WORK",
                    "value": "new_work_email@nomail.com",
                    "typeId": "EMAIL"
                },
                {
                    "id": 8557,
                    "valueType": "WORK",
                    "value": "+493012345678",
                    "typeId": "PHONE"
                },
                {
                    "id": 8559,
                    "valueType": "MOBILE",
                    "value": "+493055544433",
                    "typeId": "PHONE"
                }
            ]
        }
    }
}
```

## Errors and Diagnostics

If the method returned an error, check the request data.

#|
|| **Code** | **Cause and Action** ||
|| `NOT_FOUND` | The item is not found. Check the `id` of the contact: it could have been deleted or belong to another object type. The same code is returned when `entityTypeId` holds the identifier of a non-existent SPA ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | `entityTypeId` holds a value that matches no CRM object. A contact requires `3`, a company — `4`, a lead — `1` ||
|| `ACCESS_DENIED` | The webhook user does not have permission to modify items of the object with this `entityTypeId`. Check which user the webhook was created on behalf of ||
|| `allowed_only_intranet_user` | The webhook is created on behalf of an external user. The scenario is available to Bitrix24 employees only ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | An invalid field value. The error text names the field that failed validation ||
|#

Check separately the cases where the method responds successfully but the result is not the one you expected.

- The value has not changed and there is no error — `typeId` is not passed in the element of the `fm` object. Add it and repeat step 3

- A new entry appeared instead of a change — the contact has no entry with the `id` you passed. In this case the method does not refuse but creates a new entry. Compare the `id` with the response of step 2

- An entry with an unknown type appeared in the card — `typeId` holds a value outside the `PHONE`, `EMAIL`, `WEB`, `IM`, `LINK` list. The method does not validate such values and retains them as they are. Delete the extra entry by its `id` with an empty `value`

Step 2 changes nothing and can be repeated any number of times. If `crm.item.update` returned an error, read the contact again with step 2, compare the set of entries, and then repeat only the operations that are missing from it.

## Key Considerations {#other-types}

- If the `fm` field is passed to `crm.item.update` as an array rather than an object, the method does not replace the set of multifields but adds new entries to the existing ones: it reads the array elements as the keys `n0`, `n1`, and so on

- Phone numbers or emails cannot be cleared entirely with a single parameter. Read the contact with step 2 and pass an empty `value` for every entry of the required type

- The [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method does not check for duplicates. Running the example again creates a second contact "Klaus Weber" with the same data. Before creating, look the contact up with the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method by phone or email

- If a string is passed in the `fm` field instead of an array, the method returns no error: the contact is created without any phone numbers or emails at all, and the `hasPhone` and `hasEmail` fields keep the value `N`

- The `crm.item.*` methods name custom fields in camelCase — `ufCrm_1723209318` instead of `UF_CRM_1723209318`. This does not affect multifields, but if the same request also changes a custom field, pass the `useOriginalUfNames`: `Y` parameter to work with the familiar names

- The scenario works the same way for a contact, a company, and a lead. For a company, pass `entityTypeId`: `4` and `title` instead of `name` and `lastName`; for a lead, pass `entityTypeId`: `1`

## Code Example {#full-example}

The script creates a contact with two email addresses and two phone numbers, reads the identifiers of the multifield entries, changes the work email and the mobile phone, deletes the personal email, and displays the resulting set of contact details.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const ENTITY_TYPE_ID = 3; // 3 — contact, company — 4, lead — 1

    async function call(method, params, requestId) {
        const result = await $b24.actions.v2.call.make({ method, params, requestId });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    // finds the id of a multifield entry by its type and value subtype
    function findId(multifields, typeId, valueType) {
        const found = multifields.find(item => item.typeId === typeId && item.valueType === valueType);
        return found ? found.id : null;
    }

    async function changeContacts() {
        try {
            // Step 1: create a contact with an email and phone numbers
            const created = await call('crm.item.add', {
                entityTypeId: ENTITY_TYPE_ID,
                fields: {
                    name: 'Klaus',
                    lastName: 'Weber',
                    fm: [
                        { typeId: 'EMAIL', valueType: 'WORK', value: 'work_email@nomail.com' },
                        { typeId: 'EMAIL', valueType: 'HOME', value: 'home_email@nomail.com' },
                        { typeId: 'PHONE', valueType: 'WORK', value: '+493012345678' },
                        { typeId: 'PHONE', valueType: 'MOBILE', value: '+493076543210' }
                    ]
                }
            }, 'item-add');
            const contactId = created.item.id;
            console.log('Contact created, id:', contactId);

            // Step 2: read the identifiers of the multifield entries
            const read = await call('crm.item.get', {
                entityTypeId: ENTITY_TYPE_ID,
                id: contactId
            }, 'item-get');
            const multifields = read.item.fm;

            const workEmailId = findId(multifields, 'EMAIL', 'WORK');
            const homeEmailId = findId(multifields, 'EMAIL', 'HOME');
            const mobilePhoneId = findId(multifields, 'PHONE', 'MOBILE');
            if (!workEmailId || !homeEmailId || !mobilePhoneId) {
                console.error('The contact does not have the required multifield entries');
                return;
            }

            // Step 3: change some values and delete others
            // typeId is required in every element, otherwise the operation is silently skipped
            const updated = await call('crm.item.update', {
                entityTypeId: ENTITY_TYPE_ID,
                id: contactId,
                fields: {
                    fm: {
                        [workEmailId]: { typeId: 'EMAIL', valueType: 'WORK', value: 'new_work_email@nomail.com' },
                        [homeEmailId]: { typeId: 'EMAIL', value: '' },
                        [mobilePhoneId]: { typeId: 'PHONE', valueType: 'MOBILE', value: '+493055544433' }
                    }
                }
            }, 'item-update');

            console.log('Contact details updated:');
            console.dir(updated.item.fm);
        } catch (error) {
            console.error('Contact details not updated:', error.message);
        }
    }

    changeContacts();
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

    $entityTypeId = 3; // 3 — contact, company — 4, lead — 1
    $item = $sb->getCRMScope()->item();

    // finds the id of a multifield entry by its type and value subtype
    $findId = static function (array $multifields, string $typeId, string $valueType) {
        foreach ($multifields as $field) {
            if ($field['typeId'] === $typeId && $field['valueType'] === $valueType) {
                return $field['id'];
            }
        }
        return null;
    };

    try {
        // Step 1: create a contact with an email and phone numbers
        $contactId = $item->add(
            $entityTypeId,
            [
                'name' => 'Klaus',
                'lastName' => 'Weber',
                'fm' => [
                    [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'work_email@nomail.com' ],
                    [ 'typeId' => 'EMAIL', 'valueType' => 'HOME', 'value' => 'home_email@nomail.com' ],
                    [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+493012345678' ],
                    [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+493076543210' ]
                ]
            ]
        )->item()->id;
        echo 'Contact created, id: ' . $contactId . PHP_EOL;

        // Step 2: read the identifiers of the multifield entries
        $multifields = $item->get($entityTypeId, $contactId)->item()->fm;

        $workEmailId = $findId($multifields, 'EMAIL', 'WORK');
        $homeEmailId = $findId($multifields, 'EMAIL', 'HOME');
        $mobilePhoneId = $findId($multifields, 'PHONE', 'MOBILE');
        if ($workEmailId === null || $homeEmailId === null || $mobilePhoneId === null) {
            echo 'The contact does not have the required multifield entries';
            return;
        }

        // Step 3: change some values and delete others
        // typeId is required in every element, otherwise the operation is silently skipped
        $updated = $item->update(
            $entityTypeId,
            $contactId,
            [
                'fm' => [
                    $workEmailId => [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'new_work_email@nomail.com' ],
                    $homeEmailId => [ 'typeId' => 'EMAIL', 'value' => '' ],
                    $mobilePhoneId => [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+493055544433' ]
                ]
            ]
        );

        echo 'Contact details updated:' . PHP_EOL;
        print_r($updated->item()->fm);
    } catch (\Throwable $e) {
        echo 'Contact details not updated: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
        )
    )

    ENTITY_TYPE_ID = 3  # 3 — contact, company — 4, lead — 1


    def find_id(multifields, type_id, value_type):
        """Finds the id of a multifield entry by its type and value subtype."""
        for field in multifields:
            if field["typeId"] == type_id and field["valueType"] == value_type:
                return field["id"]
        return None


    try:
        # Step 1: create a contact with an email and phone numbers
        created = client.crm.item.add(
            ENTITY_TYPE_ID,
            {
                "name": "Klaus",
                "lastName": "Weber",
                "fm": [
                    {"typeId": "EMAIL", "valueType": "WORK", "value": "work_email@nomail.com"},
                    {"typeId": "EMAIL", "valueType": "HOME", "value": "home_email@nomail.com"},
                    {"typeId": "PHONE", "valueType": "WORK", "value": "+493012345678"},
                    {"typeId": "PHONE", "valueType": "MOBILE", "value": "+493076543210"},
                ],
            },
        ).response.result["item"]
        contact_id = created["id"]
        print(f"Contact created, id: {contact_id}")

        # Step 2: read the identifiers of the multifield entries
        multifields = client.crm.item.get(
            ENTITY_TYPE_ID,
            contact_id,
        ).response.result["item"]["fm"]

        work_email_id = find_id(multifields, "EMAIL", "WORK")
        home_email_id = find_id(multifields, "EMAIL", "HOME")
        mobile_phone_id = find_id(multifields, "PHONE", "MOBILE")

        if not all((work_email_id, home_email_id, mobile_phone_id)):
            print("The contact does not have the required multifield entries")
        else:
            # Step 3: change some values and delete others
            # typeId is required in every element, otherwise the operation is silently skipped
            updated = client.crm.item.update(
                ENTITY_TYPE_ID,
                contact_id,
                {
                    "fm": {
                        str(work_email_id): {"typeId": "EMAIL", "valueType": "WORK", "value": "new_work_email@nomail.com"},
                        str(home_email_id): {"typeId": "EMAIL", "value": ""},
                        str(mobile_phone_id): {"typeId": "PHONE", "valueType": "MOBILE", "value": "+493055544433"},
                    },
                },
            ).response.result["item"]

            print("Contact details updated:")
            print(updated["fm"])
    except BitrixAPIError as error:
        print(f"Contact details not updated: {error}")
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/crm-item-add.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-get.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-update.md)
- [{#T}](../../../api-reference/crm/data-types.md)
- [{#T}](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md)
- [{#T}](../how-to-add-crm-objects/how-to-add-contact.md)
