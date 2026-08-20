# How to Find Duplicates in CRM by Phone and Email

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to read CRM items
>
> - [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) — a user with permission to read CRM items
> - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — a user with permission to read items of a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

Duplicates appear when the same customer — a person or a company — gets into CRM several times: through a website form, a call, and a manually created card. They can be found by a matching phone number or email address.

The [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method searches leads, contacts, and companies at once, but it returns only identifiers — without names, phone numbers, or emails. That is why the data of the found objects is requested in the second step.

As a result of the scenario, we get a table with the following columns:

- object identifier

- object type: lead, contact, or company

- title or first and last name

- phone

- email address

The scenario consists of two steps.

1. Find the identifiers of the duplicates using the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method
2. Retrieve the data of the found objects using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method

## Before You Start

- The webhook is created on behalf of a user who has permission to read leads, contacts, and companies

- The `crm` scope is selected in the webhook permissions

- You have a phone number or an email to search by. One value is enough

The webhook URL grants full access to the methods of its scope. Retain it in an environment variable and never publish it in open code.

The phone and email in the examples are `+491701234567` and `duplicate@example.com`. Replace them with your own values. The rest of the data is retrieved by the scenario from the method responses.

## Prepare the Data

We will pass the phone number and the email to the script. In the JS, PHP, and Python examples, the script asks for the values itself; in the Go example, they are set by constants. In an integration, the values are substituted by the calling code.

We will create two structures:

- `entityIDs` — the identifiers of the found leads, contacts, and companies. The keys are the same ones the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method returns: `LEAD`, `CONTACT`, `COMPANY`

- `rows` — the rows of the final table

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

   ```js
   import { B24Hook } from '@bitrix24/b24jssdk'
   import { createInterface } from 'node:readline/promises'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   const rl = createInterface({ input: process.stdin, output: process.stdout })
   const phone = await rl.question('Enter the phone number: ')
   const email = await rl.question('Enter the email: ')
   rl.close()

   const entityIDs = {
       LEAD: [],
       CONTACT: [],
       COMPANY: []
   }

   const rows = []
   ```

- PHP

   ```php
   <?php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Monolog\Logger;
   use Monolog\Handler\StreamHandler;

   $log = new Logger('b24');
   $log->pushHandler(new StreamHandler('php://stdout'));

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   $phone = readline("Enter the phone number: ");
   $email = readline("Enter the email: ");

   $entityIDs = [
       'LEAD' => [],
       'CONTACT' => [],
       'COMPANY' => []
   ];

   $rows = [];
   ```

- Python

   ```python
   from b24pysdk import BitrixWebhook, Client

   client = Client(
       BitrixWebhook(
           domain="your-domain.bitrix24.com",
           webhook_token="user_id/webhook_key",
       )
   )

   phone = input("Enter the phone number: ")
   email = input("Enter the email: ")

   entity_ids = {
       "LEAD": [],
       "CONTACT": [],
       "COMPANY": [],
   }

   rows = []
   ```

- Go

    ```go
    // The phone and email we search by. The neighbouring tabs ask the user for
    // them; here they are set by constants, because the example creates the
    // objects with these values itself.
    const (
    	phone = "+491701234567"
    	email = "duplicate@example.com"
    )

    // The identifiers of the found objects and the rows of the final table. The
    // keys are the same ones crm.duplicate.findbycomm returns.
    entityIDs := map[string][]b24.ID{"LEAD": nil, "CONTACT": nil, "COMPANY": nil}
    rows := make([]row, 0)
    ```

{% endlist %}

## 1. Find the Duplicate Objects

To find the repeating objects, we will call the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method with the following parameters:

- `type` — the communication type: `PHONE` or `EMAIL`. The method searches by one type per call, so we call it separately for the phone and for the email

- `values` — an array of values. We will pass a single value, but several values can be passed in the array at once

We will merge the identifiers from both responses into `entityIDs`, removing duplicates: the same object can be found both by phone and by email.

{% list tabs %}

- JS

   ```js
   // Merges the identifiers from the method response into the entityIDs object
   function mergeDuplicates(data) {
       for (const type of ['LEAD', 'CONTACT', 'COMPANY']) {
           if (Array.isArray(data?.[type])) {
               entityIDs[type] = [...new Set(entityIDs[type].concat(data[type]))];
           }
       }
   }

   for (const [type, value] of [['PHONE', phone], ['EMAIL', email]]) {
       if (!value) {
           continue;
       }
       const result = await $b24.actions.v2.call.make({
           method: 'crm.duplicate.findbycomm',
           params: { type, values: [value] }
       });
       if (result.isSuccess) {
           mergeDuplicates(result.getData()?.result);
       } else {
           console.error(`Error searching for duplicates by ${type}:`, result.getErrorMessages().join('; '));
       }
   }
   ```

- PHP

   ```php
   use Bitrix24\SDK\Services\CRM\Duplicates\Result\DuplicateResult;

   // Merges the identifiers from the method response into the $entityIDs array
   function mergeDuplicates(DuplicateResult $result, array &$entityIDs): void
   {
       $data = $result->getCoreResponse()->getResponseData()->getResult();
       foreach (['LEAD', 'CONTACT', 'COMPANY'] as $type) {
           if (!empty($data[$type]) && is_array($data[$type])) {
               $entityIDs[$type] = array_values(array_unique(
                   array_merge($entityIDs[$type], $data[$type])
               ));
           }
       }
   }

   if ($phone) {
       mergeDuplicates($sb->getCRMScope()->duplicate()->findByPhone([$phone]), $entityIDs);
   }

   if ($email) {
       mergeDuplicates($sb->getCRMScope()->duplicate()->findByEmail([$email]), $entityIDs);
   }
   ```

- Python

   ```python
   def merge_duplicates(data, entity_ids):
       """Merges the identifiers from the method response into entity_ids."""
       if not isinstance(data, dict):
           return
       for key in entity_ids:
           found = data.get(key)
           if isinstance(found, list):
               entity_ids[key] = list(dict.fromkeys(entity_ids[key] + found))


   for comm_type, value in (("PHONE", phone), ("EMAIL", email)):
       if not value:
           continue
       result = client.crm.duplicate.findbycomm(
           type=comm_type,
           values=[value],
       ).response.result
       merge_duplicates(result, entity_ids)
   ```

- Go

    ```go
    // The method searches by ONE communication type per call, so we query the
    // phone and the email separately and accumulate the identifiers in a shared map.
    for _, comm := range []struct{ typ, value string }{
    	{"PHONE", phone},
    	{"EMAIL", email},
    } {
    	if comm.value == "" {
    		continue
    	}
    	res, err := core.Call(ctx, "crm.duplicate.findbycomm", b24.Params{
    		"type":   comm.typ,
    		"values": []string{comm.value},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.duplicate.findbycomm %s: %w", comm.typ, err)
    	}

    	// The response is an object with the LEAD, CONTACT, and COMPANY keys. A key
    	// may be missing entirely: if nothing was found for that type, it is simply
    	// not sent. When nothing was found at all, result comes as an empty array
    	// rather than an object, so we ignore the parsing error here.
    	var found map[string][]b24.ID
    	if err := json.Unmarshal(res.Result, &found); err == nil {
    		for key := range entityIDs {
    			entityIDs[key] = appendUnique(entityIDs[key], found[key])
    		}
    	}
    }
    ```

{% endlist %}

The method returns the identifiers of the objects in which the phone or the email was found. The response holds only the keys for which something was found.

```json
{
    "result": {
        "LEAD": [1001149],
        "CONTACT": [2693],
        "COMPANY": [3013]
    }
}
```

{% note warning "" %}

If nothing was found, the method returns `result` as an empty array rather than an object with empty keys:

```json
{
    "result": []
}
```

Code that addresses `result.LEAD` right away crashes on such a response. Check the type of the value before addressing the keys.

{% endnote %}

## 2. Retrieve the Data of the Found Objects

The data of all three object types is returned by a single method — [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md). We will call it for every non-empty list of identifiers with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier. The values are returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method: `1` — a lead, `3` — a contact, `4` — a company

- `filter[id]` — the array of identifiers from step 1

- `select` — the fields to return. We will specify `id`, `title`, `name`, `lastName`, `phone`, and `email`. The same list suits all three types: the fields an object does not have are discarded by the method

{% list tabs %}

- JS

   ```js
   const SELECT = ['id', 'title', 'name', 'lastName', 'phone', 'email'];
   const ENTITY_TYPES = [
       { key: 'LEAD', entityTypeId: 1, label: 'lead' },
       { key: 'CONTACT', entityTypeId: 3, label: 'contact' },
       { key: 'COMPANY', entityTypeId: 4, label: 'company' }
   ];

   for (const type of ENTITY_TYPES) {
       if (entityIDs[type.key].length === 0) {
           continue;
       }
       const result = await $b24.actions.v2.call.make({
           method: 'crm.item.list',
           params: {
               entityTypeId: type.entityTypeId,
               filter: { id: entityIDs[type.key] },
               select: SELECT
           }
       });
       if (!result.isSuccess) {
           console.error(result.getErrorMessages().join('; '));
           continue;
       }
       for (const item of result.getData().result.items) {
           const name = [item.name, item.lastName].filter(Boolean).join(' ');
           rows.push({
               id: item.id,
               kind: type.label,
               title: name || item.title || '—',
               phone: item.phone || '—',
               email: item.email || '—'
           });
       }
   }
   ```

- PHP

   ```php
   $select = ['id', 'title', 'name', 'lastName', 'phone', 'email'];
   $entityTypes = [
       ['key' => 'LEAD', 'entityTypeId' => 1, 'label' => 'lead'],
       ['key' => 'CONTACT', 'entityTypeId' => 3, 'label' => 'contact'],
       ['key' => 'COMPANY', 'entityTypeId' => 4, 'label' => 'company'],
   ];

   foreach ($entityTypes as $type) {
       if (empty($entityIDs[$type['key']])) {
           continue;
       }

       $items = $sb->getCRMScope()->item()->list(
           $type['entityTypeId'],
           [],
           ['id' => $entityIDs[$type['key']]],
           $select
       )->getItems();

       foreach ($items as $item) {
           $name = trim(($item->name ?? '') . ' ' . ($item->lastName ?? ''));
           $rows[] = [
               'id' => $item->id,
               'kind' => $type['label'],
               'title' => $name ?: ($item->title ?? '—'),
               'phone' => $item->phone ?: '—',
               'email' => $item->email ?: '—',
           ];
       }
   }
   ```

- Python

   ```python
   SELECT = ["id", "title", "name", "lastName", "phone", "email"]
   ENTITY_TYPES = (
       ("LEAD", 1, "lead"),
       ("CONTACT", 3, "contact"),
       ("COMPANY", 4, "company"),
   )

   for key, entity_type_id, label in ENTITY_TYPES:
       if not entity_ids[key]:
           continue

       items = client.crm.item.list(
           entity_type_id=entity_type_id,
           filter={"id": entity_ids[key]},
           select=SELECT,
       ).response.result["items"]

       for item in items:
           name = " ".join(filter(None, [item.get("name"), item.get("lastName")]))
           rows.append({
               "id": item["id"],
               "kind": label,
               "title": name or item.get("title") or "—",
               "phone": item.get("phone") or "—",
               "email": item.get("email") or "—",
           })
   ```

- Go

    ```go
    // The data of all three types is returned by one method: only entityTypeId differs.
    for _, spec := range entityTypes {
    	ids := entityIDs[spec.key]
    	if len(ids) == 0 {
    		continue
    	}
    	res, err := core.Call(ctx, "crm.item.list", b24.Params{
    		"entityTypeId": spec.entityTypeID,
    		"filter":       b24.Params{"id": ids},
    		"select":       []string{"id", "title", "name", "lastName", "phone", "email"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.item.list %s: %w", spec.key, err)
    	}

    	// The method wraps the response in an object with the items key, fields in camelCase.
    	var list struct {
    		Items []entity `json:"items"`
    	}
    	if err := json.Unmarshal(res.Result, &list); err != nil {
    		return fmt.Errorf("parse the %s response: %w", spec.key, err)
    	}
    	for _, e := range list.Items {
    		rows = append(rows, e.row(spec.label))
    	}
    }
    ```

{% endlist %}

The method returns the objects matching the filter. Below is the response for a lead: it has both `title` and a first and last name. A contact has no `title` field, and a company has no `name` and `lastName`.

```json
{
    "result": {
        "items": [
            {
                "id": 1001149,
                "title": "Website request",
                "name": "Klaus",
                "lastName": "Weber",
                "email": "duplicate@example.com",
                "phone": "+491701234567"
            }
        ]
    },
    "total": 1
}
```

The `phone` and `email` fields come as strings — this is the first value from the card. If an object has several phone numbers or addresses, the rest are kept in the multiple field `fm`. It comes only when `select` is not passed or is specified as `["*"]`. `fm` cannot be picked as a separate field in `select`.

## Final Table

We will assemble the accumulated `rows` into a table.

{% list tabs %}

- JS

   ```js
   if (rows.length === 0) {
       console.log('No duplicates found');
   } else {
       console.table(rows);
   }
   ```

- PHP

   ```php
   if (empty($rows)) {
       echo "No duplicates found\n";
   } else {
       echo implode("\t", ['ID', 'Object type', 'Title/First and last name', 'Phone', 'Email']) . "\n";
       foreach ($rows as $row) {
           echo implode("\t", $row) . "\n";
       }
   }
   ```

- Python

   ```python
   if not rows:
       print("No duplicates found")
   else:
       print("\t".join(["ID", "Object type", "Title/First and last name", "Phone", "Email"]))
       for row in rows:
           print("\t".join(str(row[key]) for key in ("id", "kind", "title", "phone", "email")))
   ```

- Go

    ```go
    if len(rows) == 0 {
    	fmt.Println("No duplicates found")
    	return nil
    }
    fmt.Println("ID\tObject type\tTitle/First and last name\tPhone\tEmail")
    for _, r := range rows {
    	fmt.Printf("%d\t%s\t%s\t%s\t%s\n", r.ID, r.Kind, r.Title, r.Phone, r.Email)
    }
    ```

{% endlist %}

## Verify the Result

The scenario is complete if the table has a row for every identifier from step 1.

What to check:

- The number of rows matches the sum of the lengths of the `LEAD`, `CONTACT`, and `COMPANY` lists from step 1. If there are fewer rows, some objects are not available to the webhook user due to permissions

- The `Phone` and `Email` columns hold the values that were searched for. If a row holds a different number, the object matched by email, or it has several phone numbers and only the first one comes in the response

A single row in the table is not an error but the absence of duplicates: the value occurs in CRM only once.

You can check the result in the interface by searching for the phone number in the Bitrix24 search bar: the results will hold the same leads, contacts, and companies.

## Errors and Diagnostics

If the method returned an error, check the request data.

#|
|| **Code** | **Cause and Action** ||
|| `Communication values is not defined` | In step 1, a string was passed in `values` instead of an array. The parameter accepts an array even when there is a single value ||
|| `403` `Access denied` | The webhook user has no permission to read CRM items. Check which user the webhook was created on behalf of ||
|| `NOT_FOUND` | In step 2, `entityTypeId` holds a value that matches no CRM object. It requires `1`, `3`, or `4` ||
|| `INVALID_ARG_VALUE` `Invalid filter: field 'field' is not allowed in filter` | In step 2, the `filter` holds a field that cannot be filtered by. The list of available fields is returned by the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method ||
|#

An empty result is not considered an error.

- An empty `result` in step 1 — there are no objects with such a phone or email. A possible reason is the number format, see the "Key Considerations" section below

- An empty `items` in step 2 with a non-empty step 1 — the objects were found but are not available to the webhook user due to permissions. Repeat step 2 with an administrator webhook to confirm this

Both methods only read data, so after an error the scenario can be repeated from any step.

## Key Considerations

- The [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method compares the number as a whole, ignoring only the extension. Records with a number in a different format are not considered duplicates, so bring the numbers to a single form when retaining them in CRM

- If 20 or more duplicates were found for one object type — leads, contacts, or companies — the method does not return the other types at all. With 20 duplicate leads, the response holds only the `LEAD` key, and the contacts and companies with the same phone number silently disappear. To retrieve them, repeat the call with the `entity_type` parameter: `CONTACT` or `COMPANY`

- The [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method returns no more than 50 items per call. If there are more than 50 identifiers of one type, iterate over the pages with the `start` parameter

- The found duplicates can be merged with the [crm.entity.mergebatch](../../../api-reference/crm/duplicates/crm-entity-merge-batch.md) method

## Code Example

The code goes through both steps and prints a table of duplicates. The webhook URL has to be replaced: in the JS example it is read from an environment variable, in PHP and Python it is set directly in the code. The JS, PHP, and Python examples ask for the phone and the email at startup; in the Go example they are set by constants.

{% list tabs %}

- JS

   ```js
   import { B24Hook } from '@bitrix24/b24jssdk'
   import { createInterface } from 'node:readline/promises'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   const SELECT = ['id', 'title', 'name', 'lastName', 'phone', 'email']
   const ENTITY_TYPES = [
       { key: 'LEAD', entityTypeId: 1, label: 'lead' },
       { key: 'CONTACT', entityTypeId: 3, label: 'contact' },
       { key: 'COMPANY', entityTypeId: 4, label: 'company' }
   ]

   // Asking the user for the phone and the email
   const rl = createInterface({ input: process.stdin, output: process.stdout })
   const phone = await rl.question('Enter the phone number: ')
   const email = await rl.question('Enter the email: ')
   rl.close()

   const entityIDs = { LEAD: [], CONTACT: [], COMPANY: [] }
   const rows = []

   // Merges the identifiers from the method response into the entityIDs object
   function mergeDuplicates(data) {
       for (const type of ['LEAD', 'CONTACT', 'COMPANY']) {
           if (Array.isArray(data?.[type])) {
               entityIDs[type] = [...new Set(entityIDs[type].concat(data[type]))]
           }
       }
   }

   // Step 1: Searching for duplicates by phone and by email
   for (const [type, value] of [['PHONE', phone], ['EMAIL', email]]) {
       if (!value) {
           continue
       }
       const result = await $b24.actions.v2.call.make({
           method: 'crm.duplicate.findbycomm',
           params: { type, values: [value] }
       })
       if (result.isSuccess) {
           mergeDuplicates(result.getData()?.result)
       } else {
           console.error(`Error searching for duplicates by ${type}:`, result.getErrorMessages().join('; '))
       }
   }

   // Step 2: Retrieving the data of the found objects
   for (const type of ENTITY_TYPES) {
       if (entityIDs[type.key].length === 0) {
           continue
       }
       const result = await $b24.actions.v2.call.make({
           method: 'crm.item.list',
           params: {
               entityTypeId: type.entityTypeId,
               filter: { id: entityIDs[type.key] },
               select: SELECT
           }
       })
       if (!result.isSuccess) {
           console.error(result.getErrorMessages().join('; '))
           continue
       }
       for (const item of result.getData().result.items) {
           const name = [item.name, item.lastName].filter(Boolean).join(' ')
           rows.push({
               id: item.id,
               kind: type.label,
               title: name || item.title || '—',
               phone: item.phone || '—',
               email: item.email || '—'
           })
       }
   }

   // Printing the table to the console
   if (rows.length === 0) {
       console.log('No duplicates found')
   } else {
       console.table(rows)
   }
   ```

- PHP

   ```php
   <?php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Bitrix24\SDK\Services\CRM\Duplicates\Result\DuplicateResult;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Monolog\Logger;
   use Monolog\Handler\StreamHandler;

   $log = new Logger('b24');
   $log->pushHandler(new StreamHandler('php://stdout'));

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   // Asking the user for the phone and the email
   $phone = readline("Enter the phone number: ");
   $email = readline("Enter the email: ");

   $entityIDs = ['LEAD' => [], 'CONTACT' => [], 'COMPANY' => []];
   $rows = [];

   $select = ['id', 'title', 'name', 'lastName', 'phone', 'email'];
   $entityTypes = [
       ['key' => 'LEAD', 'entityTypeId' => 1, 'label' => 'lead'],
       ['key' => 'CONTACT', 'entityTypeId' => 3, 'label' => 'contact'],
       ['key' => 'COMPANY', 'entityTypeId' => 4, 'label' => 'company'],
   ];

   // Merges the identifiers from the method response into the $entityIDs array
   function mergeDuplicates(DuplicateResult $result, array &$entityIDs): void
   {
       $data = $result->getCoreResponse()->getResponseData()->getResult();
       foreach (['LEAD', 'CONTACT', 'COMPANY'] as $type) {
           if (!empty($data[$type]) && is_array($data[$type])) {
               $entityIDs[$type] = array_values(array_unique(
                   array_merge($entityIDs[$type], $data[$type])
               ));
           }
       }
   }

   try {
       // Step 1: Searching for duplicates by phone and by email
       if ($phone) {
           mergeDuplicates($sb->getCRMScope()->duplicate()->findByPhone([$phone]), $entityIDs);
       }

       if ($email) {
           mergeDuplicates($sb->getCRMScope()->duplicate()->findByEmail([$email]), $entityIDs);
       }

       // Step 2: Retrieving the data of the found objects
       foreach ($entityTypes as $type) {
           if (empty($entityIDs[$type['key']])) {
               continue;
           }

           $items = $sb->getCRMScope()->item()->list(
               $type['entityTypeId'],
               [],
               ['id' => $entityIDs[$type['key']]],
               $select
           )->getItems();

           foreach ($items as $item) {
               $name = trim(($item->name ?? '') . ' ' . ($item->lastName ?? ''));
               $rows[] = [
                   'id' => $item->id,
                   'kind' => $type['label'],
                   'title' => $name ?: ($item->title ?? '—'),
                   'phone' => $item->phone ?: '—',
                   'email' => $item->email ?: '—',
               ];
           }
       }
   } catch (\Throwable $e) {
       echo $e->getMessage() . "\n";
   }

   // Printing the table with tab separators
   if (empty($rows)) {
       echo "No duplicates found\n";
   } else {
       echo implode("\t", ['ID', 'Object type', 'Title/First and last name', 'Phone', 'Email']) . "\n";
       foreach ($rows as $row) {
           echo implode("\t", $row) . "\n";
       }
   }
   ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    SELECT = ["id", "title", "name", "lastName", "phone", "email"]
    ENTITY_TYPES = (
        ("LEAD", 1, "lead"),
        ("CONTACT", 3, "contact"),
        ("COMPANY", 4, "company"),
    )


    def merge_duplicates(data, entity_ids):
        """Merges the identifiers from the method response into entity_ids."""
        if not isinstance(data, dict):
            return
        for key in entity_ids:
            found = data.get(key)
            if isinstance(found, list):
                entity_ids[key] = list(dict.fromkeys(entity_ids[key] + found))


    phone = input("Enter the phone number: ")
    email = input("Enter the email: ")

    entity_ids = {"LEAD": [], "CONTACT": [], "COMPANY": []}
    rows = []

    try:
        # Step 1: Searching for duplicates by phone and by email
        for comm_type, value in (("PHONE", phone), ("EMAIL", email)):
            if not value:
                continue
            result = client.crm.duplicate.findbycomm(
                type=comm_type,
                values=[value],
            ).response.result
            merge_duplicates(result, entity_ids)

        # Step 2: Retrieving the data of the found objects
        for key, entity_type_id, label in ENTITY_TYPES:
            if not entity_ids[key]:
                continue

            items = client.crm.item.list(
                entity_type_id=entity_type_id,
                filter={"id": entity_ids[key]},
                select=SELECT,
            ).response.result["items"]

            for item in items:
                name = " ".join(filter(None, [item.get("name"), item.get("lastName")]))
                rows.append({
                    "id": item["id"],
                    "kind": label,
                    "title": name or item.get("title") or "—",
                    "phone": item.get("phone") or "—",
                    "email": item.get("email") or "—",
                })
    except BitrixAPIError as error:
        print(error)

    # Printing the table with tab separators
    if not rows:
        print("No duplicates found")
    else:
        print("\t".join(["ID", "Object type", "Title/First and last name", "Phone", "Email"]))
        for row in rows:
            print("\t".join(str(row[key]) for key in ("id", "kind", "title", "phone", "email")))
    ```

- Go

    ```go
    // Setup in an empty directory — go get does not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-domain.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates a lead, a contact, and a company
    // with the same phone and email, finds them as duplicates, prints the table,
    // and cleans up after itself. It runs on any Bitrix24, nothing has to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"
    	"strings"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // The phone and email we search by. The neighbouring tabs ask the user for
    // them; here they are set by constants, because the example creates the
    // objects with these values itself.
    const (
    	phone = "+491701234567"
    	email = "duplicate@example.com"
    )

    // The object types we search duplicates in. The key is the same one the
    // crm.duplicate.findbycomm response holds, entityTypeID comes from crm.enum.ownertype.
    var entityTypes = []struct {
    	key          string
    	entityTypeID int
    	label        string
    }{
    	{"LEAD", 1, "lead"},
    	{"CONTACT", 3, "contact"},
    	{"COMPANY", 4, "company"},
    }

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// The identifiers of the found objects and the rows of the final table. The
    	// keys are the same ones crm.duplicate.findbycomm returns.
    	entityIDs := map[string][]b24.ID{"LEAD": nil, "CONTACT": nil, "COMPANY": nil}
    	rows := make([]row, 0)

    	// --- preparation: our own duplicates

    	cleanup, err := createDuplicates(ctx, core, phone, email)
    	defer cleanup()
    	if err != nil {
    		return err
    	}

    	// --- step 1: searching for duplicates by communications
    	// The method searches by ONE communication type per call, so we query the
    	// phone and the email separately and accumulate the identifiers in a shared map.
    	for _, comm := range []struct{ typ, value string }{
    		{"PHONE", phone},
    		{"EMAIL", email},
    	} {
    		if comm.value == "" {
    			continue
    		}
    		res, err := core.Call(ctx, "crm.duplicate.findbycomm", b24.Params{
    			"type":   comm.typ,
    			"values": []string{comm.value},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.duplicate.findbycomm %s: %w", comm.typ, err)
    		}

    		// The response is an object with the LEAD, CONTACT, and COMPANY keys. A key
    		// may be missing entirely: if nothing was found for that type, it is simply
    		// not sent. When nothing was found at all, result comes as an empty array
    		// rather than an object, so we ignore the parsing error here.
    		var found map[string][]b24.ID
    		if err := json.Unmarshal(res.Result, &found); err == nil {
    			for key := range entityIDs {
    				entityIDs[key] = appendUnique(entityIDs[key], found[key])
    			}
    		}
    	}
    	fmt.Printf("found: leads %d, contacts %d, companies %d\n",
    		len(entityIDs["LEAD"]), len(entityIDs["CONTACT"]), len(entityIDs["COMPANY"]))

    	// --- step 2: the data of the found objects
    	// The data of all three types is returned by one method: only entityTypeId differs.
    	for _, spec := range entityTypes {
    		ids := entityIDs[spec.key]
    		if len(ids) == 0 {
    			continue
    		}
    		res, err := core.Call(ctx, "crm.item.list", b24.Params{
    			"entityTypeId": spec.entityTypeID,
    			"filter":       b24.Params{"id": ids},
    			"select":       []string{"id", "title", "name", "lastName", "phone", "email"},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.item.list %s: %w", spec.key, err)
    		}

    		// The method wraps the response in an object with the items key, fields in camelCase.
    		var list struct {
    			Items []entity `json:"items"`
    		}
    		if err := json.Unmarshal(res.Result, &list); err != nil {
    			return fmt.Errorf("parse the %s response: %w", spec.key, err)
    		}
    		for _, e := range list.Items {
    			rows = append(rows, e.row(spec.label))
    		}
    	}

    	// --- printing the table
    	if len(rows) == 0 {
    		fmt.Println("No duplicates found")
    		return nil
    	}
    	fmt.Println("ID\tObject type\tTitle/First and last name\tPhone\tEmail")
    	for _, r := range rows {
    		fmt.Printf("%d\t%s\t%s\t%s\t%s\n", r.ID, r.Kind, r.Title, r.Phone, r.Email)
    	}
    	return nil
    }

    // entity — the common shape of a crm.item.list response row: the set of fields
    // differs for a lead, a contact, and a company, but the ones we need coincide.
    type entity struct {
    	ID       b24.ID `json:"id"`
    	Title    string `json:"title"`
    	Name     string `json:"name"`
    	LastName string `json:"lastName"`
    	Phone    string `json:"phone"`
    	Email    string `json:"email"`
    }

    type row struct {
    	ID                        b24.ID
    	Kind, Title, Phone, Email string
    }

    func (e entity) row(kind string) row {
    	title := strings.TrimSpace(e.Name + " " + e.LastName)
    	if title == "" {
    		title = e.Title
    	}
    	return row{ID: e.ID, Kind: kind, Title: title,
    		Phone: orDash(e.Phone), Email: orDash(e.Email)}
    }

    func orDash(value string) string {
    	if value == "" {
    		return "—"
    	}
    	return value
    }

    func appendUnique(dst, src []b24.ID) []b24.ID {
    	seen := make(map[b24.ID]bool, len(dst))
    	for _, id := range dst {
    		seen[id] = true
    	}
    	for _, id := range src {
    		if !seen[id] {
    			seen[id] = true
    			dst = append(dst, id)
    		}
    	}
    	return dst
    }

    // --- auxiliary: data preparation and cleanup

    // createDuplicates creates a lead, a contact, and a company with the same phone
    // and email — exactly the situation the scenario searches for. It returns a
    // cleanup function: it is called even when the preparation broke halfway.
    func createDuplicates(ctx context.Context, core *b24.Core, phone, email string) (func(), error) {
    	comm := b24.Params{
    		"PHONE": []map[string]any{b24.MultifieldAdd(phone, "WORK")},
    		"EMAIL": []map[string]any{b24.MultifieldAdd(email, "WORK")},
    	}
    	created := map[string]b24.ID{}
    	cleanup := func() {
    		for method, id := range created {
    			del(ctx, core, method, b24.Params{"id": id})
    		}
    	}

    	for _, spec := range []struct {
    		add, delete string
    		fields      b24.Params
    	}{
    		{"crm.lead.add", "crm.lead.delete", b24.Params{"TITLE": "Website request", "NAME": "Klaus", "LAST_NAME": "Weber"}},
    		{"crm.contact.add", "crm.contact.delete", b24.Params{"NAME": "Klaus", "LAST_NAME": "Weber"}},
    		{"crm.company.add", "crm.company.delete", b24.Params{"TITLE": "Müller GmbH"}},
    	} {
    		fields := b24.Params{}
    		for k, v := range spec.fields {
    			fields[k] = v
    		}
    		for k, v := range comm {
    			fields[k] = v
    		}
    		res, err := core.Call(ctx, spec.add, b24.Params{"fields": fields})
    		if err != nil {
    			return cleanup, fmt.Errorf("%s: %w", spec.add, err)
    		}
    		var id b24.ID
    		if err := json.Unmarshal(res.Result, &id); err != nil {
    			return cleanup, fmt.Errorf("parse the identifier from %s: %w", spec.add, err)
    		}
    		created[spec.delete] = id
    	}
    	return cleanup, nil
    }

    // del removes what was created. The cleanup error is printed but not returned:
    // it must not replace the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v\n", method, err)
    	}
    }
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md)
- [{#T}](../../../api-reference/crm/duplicates/crm-entity-merge-batch.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-list.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)
- [{#T}](./how-to-get-elements-by-stage-filter.md)
