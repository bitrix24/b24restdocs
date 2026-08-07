# How to Find Duplicates in CRM by Phone and Email

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute methods: users with read access to CRM entities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

You can automate the search for duplicates by phone and email address using a script. It will find leads, contacts, and companies with matching data, retrieve information about them, and display it in a table:

- object identifier,

- object type: lead, contact, or company,

- heading or first and last name,

- phone,

- email address.

To find duplicates, we will call the following methods sequentially:

1. [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) — find duplicates by phone and email,

2. [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) — retrieve leads,

3. [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) — retrieve contacts,

4. [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) — retrieve companies.

## Prepare the Data

We will pass the phone number and email to the script using dialog boxes in the browser. The values will be stored in the variables `phone` and `email`.

If the data is entered correctly and duplicates are found, they will be displayed in the table. Otherwise, the table will be empty.

We will create arrays:

- `entityIDs` — identifiers of the found leads, contacts, and companies,

- `$resultEntity` — detailed data about the found objects.

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

   ```js
   let phone = prompt("Enter phone number:");
   let email = prompt("Enter email:");
   
   let entityIDs = {
       'LEAD': [],
       'CONTACT': [],
       'COMPANY': []
   };
   
   let resultEntity = {
       'lead': [],
       'contact': [],
       'company': []
   };
   ```

- PHP

   ```php
   $phone = readline("Enter phone number: ");
   $email = readline("Enter email: ");
   
   $entityIDs = [
       'LEAD' => [],
       'CONTACT' => [],
       'COMPANY' => []
   ];
   $resultEntity = [
       'lead' => [],
       'contact' => [],
       'company' => []
   ];
   ```

- Python

   ```python
   phone = input("Enter phone number: ")
   email = input("Enter email: ")

   entity_ids = {
       "LEAD": [],
       "CONTACT": [],
       "COMPANY": [],
   }

   result_entity = {
       "lead": [],
       "contact": [],
       "company": [],
   }
   ```

- Go

    ```go
    // The phone and the email to search by. Neighbouring tabs ask them from
    // the user; here they are set by constants, because the example itself creates
    // objects with these values.
    const (
    	phone = "+49 900 555-35-35"
    	email = "duplicate@example.com"
    )

    // The IDs of the found objects and their detailed data. The keys are
    // the same ones that crm.duplicate.findbycomm returns.
    entityIDs := map[string][]b24.ID{"LEAD": nil, "CONTACT": nil, "COMPANY": nil}
    rows := make([]row, 0)
    ```

{% endlist %}

## 1. Find Duplicate Objects

To find duplicate objects by phone and email, we will call the method [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) twice. We will pass two parameters to it.

- `type` — communication type, `PHONE` or `EMAIL`.

- `values` — an array of phone numbers or email addresses. We will specify variables `phone` and `email`.

We will combine the identifiers of the found duplicates into the `entityIDs` array.

{% list tabs %}

- JS

   ```js
   // Merges identifiers from the method response with the entityIDs object
   function mergeDuplicates(data) {
       for (const type of ['LEAD', 'CONTACT', 'COMPANY']) {
           if (Array.isArray(data?.[type])) {
               entityIDs[type] = entityIDs[type].concat(data[type]);
           }
       }
   }

   if (phone) {
       const phoneResult = await $b24.actions.v2.call.make({
           method: 'crm.duplicate.findbycomm',
           params: { type: 'PHONE', values: [phone] }
       });
       if (phoneResult.isSuccess) {
           mergeDuplicates(phoneResult.getData()?.result);
       } else {
           console.error('Error searching for duplicates by phone:', phoneResult.getErrorMessages().join('; '));
       }
   }

   if (email) {
       const emailResult = await $b24.actions.v2.call.make({
           method: 'crm.duplicate.findbycomm',
           params: { type: 'EMAIL', values: [email] }
       });
       if (emailResult.isSuccess) {
           mergeDuplicates(emailResult.getData()?.result);
       } else {
           console.error('Error searching for duplicates by email:', emailResult.getErrorMessages().join('; '));
       }
   }
   ```

- PHP

   ```php
   use Bitrix24\SDK\Services\CRM\Duplicates\Result\DuplicateResult;

   // Merges identifiers from the method response with the $entityIDs array
   function mergeDuplicates(DuplicateResult $result, array &$entityIDs): void
   {
       $data = $result->getCoreResponse()->getResponseData()->getResult();
       foreach (['LEAD', 'CONTACT', 'COMPANY'] as $type) {
           if (!empty($data[$type]) && is_array($data[$type])) {
               $entityIDs[$type] = array_merge($entityIDs[$type], $data[$type]);
           }
       }
   }

   if($phone)
   {
       mergeDuplicates($sb->getCRMScope()->duplicate()->findByPhone([$phone]), $entityIDs);
   }

   if($email)
   {
       mergeDuplicates($sb->getCRMScope()->duplicate()->findByEmail([$email]), $entityIDs);
   }
   ```

- Python

   ```python
   if phone:
       result = client.crm.duplicate.findbycomm(
           type="PHONE",
           values=[phone],
       ).result
       for key in entity_ids:
           if isinstance(result.get(key), list):
               entity_ids[key].extend(result[key])

   if email:
       result = client.crm.duplicate.findbycomm(
           type="EMAIL",
           values=[email],
       ).result
       for key in entity_ids:
           if isinstance(result.get(key), list):
               entity_ids[key].extend(result[key])
   ```

- Go

    ```go
    // The method searches by ONE communication type per call, so the phone and the email are
    // queried separately, and the IDs are accumulated in a shared map.
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

    	// The response is an object with the LEAD, CONTACT, and COMPANY keys. A key may be absent
    	// at all: if nothing was found for that type, it is simply not sent.
    	var found map[string][]b24.ID
    	if err := json.Unmarshal(res.Result, &found); err != nil {
    		return fmt.Errorf("parse duplicates: %w", err)
    	}
    	for key := range entityIDs {
    		entityIDs[key] = appendUnique(entityIDs[key], found[key])
    	}
    }
    ```

{% endlist %}

The [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method returns lists of identifiers for leads, contacts, and companies where the specified phone number or email address is found.

## 2. Retrieve Leads

If the list of lead identifiers is not empty, we will retrieve their data using the [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) method.

1. Apply a filter by identifier.

2. Select the fields: `ID`, `NAME`, `LAST_NAME`, `PHONE`, `EMAIL`, `TITLE`.

3. Save the result in the `resultEntity` array.

{% list tabs %}

- JS

   ```js
   if (entityIDs.LEAD.length > 0) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.lead.list',
           params: {
               filter: { ID: entityIDs.LEAD },
               select: ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE']
           }
       });
       if (result.isSuccess) {
           const leads = result.getData()?.result;
           if (leads?.length > 0) {
               resultEntity.lead = leads;
           }
       } else {
           console.error(result.getErrorMessages().join('; '));
       }
   }
   ```

- PHP

   ```php
   if(!empty($entityIDs['LEAD']))
   {
       $leads = $sb->getCRMScope()->lead()->list(
           [],
           ['ID' => $entityIDs['LEAD']],
           ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE']
       )->getLeads();
       if(!empty($leads))
       {
           $resultEntity['lead'] = $leads;
       }
   }
   ```

- Python

   ```python
   if entity_ids["LEAD"]:
       result = client.crm.lead.list(
           filter={"ID": entity_ids["LEAD"]},
           select=["ID", "NAME", "LAST_NAME", "PHONE", "EMAIL", "TITLE"],
       ).result

       if result:
           result_entity["lead"] = result
   ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.lead.list", b24.Params{
    	"filter": b24.Params{"ID": entityIDs["LEAD"]},
    	"select": []string{"ID", "NAME", "LAST_NAME", "PHONE", "EMAIL", "TITLE"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.lead.list: %w", err)
    }
    var leads []entity
    if err := json.Unmarshal(res.Result, &leads); err != nil {
    	return fmt.Errorf("parse leads: %w", err)
    }
    for _, e := range leads {
    	rows = append(rows, e.row("lead"))
    }
    ```

{% endlist %}

The method [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) will return a list of leads based on the filter.

```json
{
    "result":[{
        "ID":"1183",
        "NAME":null,
        "LAST_NAME":null,
        "TITLE":"Filling out the CRM form \"Contact data form for open lines\"",
        "PHONE":[{
            "ID":"1957",
            "VALUE_TYPE":"OTHER",
            "VALUE":"+493216464646",
            "TYPE_ID":"PHONE"
        }]
    }]
}
```

## 3. Retrieve Contacts

If the list of contact IDs is not empty, we will retrieve their data using the method [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md).

1. Apply a filter by identifier.

2. Select the fields: `ID`, `NAME`, `LAST_NAME`, `PHONE`, `EMAIL`.

3. Save the result in the `resultEntity` array.

{% list tabs %}

- JS

   ```js
   if (entityIDs.CONTACT.length > 0) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.contact.list',
           params: {
               filter: { ID: entityIDs.CONTACT },
               select: ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL']
           }
       });
       if (result.isSuccess) {
           const contacts = result.getData()?.result;
           if (contacts?.length > 0) {
               resultEntity.contact = contacts;
           }
       } else {
           console.error(result.getErrorMessages().join('; '));
       }
   }
   ```

- PHP

   ```php
   if(!empty($entityIDs['CONTACT']))
   {
       $contacts = $sb->getCRMScope()->contact()->list(
           [],
           ['ID' => $entityIDs['CONTACT']],
           ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL'],
           0
       )->getContacts();
       if(!empty($contacts))
       {
           $resultEntity['contact'] = $contacts;
       }
   }
   ```

- Python

   ```python
   if entity_ids["CONTACT"]:
       result = client.crm.contact.list(
           filter={"ID": entity_ids["CONTACT"]},
           select=["ID", "NAME", "LAST_NAME", "PHONE", "EMAIL"],
       ).result

       if result:
           result_entity["contact"] = result
   ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.contact.list", b24.Params{
    	"filter": b24.Params{"ID": entityIDs["CONTACT"]},
    	"select": []string{"ID", "NAME", "LAST_NAME", "PHONE", "EMAIL"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.list: %w", err)
    }
    var contacts []entity
    if err := json.Unmarshal(res.Result, &contacts); err != nil {
    	return fmt.Errorf("parse contacts: %w", err)
    }
    for _, e := range contacts {
    	rows = append(rows, e.row("contact"))
    }
    ```

{% endlist %}

The [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) method returns a list of contacts by filter.

```json
{
    "result":[{
        "ID":"23",
        "NAME":"Klaus",
        "LAST_NAME":"Weber",
        "EMAIL":[{
            "ID":"854",
            "VALUE_TYPE":"WORK",
            "VALUE":"alekseev@ya.com",
            "TYPE_ID":"EMAIL"
        }]
    }]
}
```

## 4. Retrieve Companies

If the list of company IDs is not empty, we will retrieve their data using the method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md).

1. Apply a filter by identifier.

2. Select the following fields: `ID`, `PHONE`, `EMAIL`, `TITLE`.

3. Retain the result in the `resultEntity` array.

{% list tabs %}

- JS

   ```js
   if (entityIDs.COMPANY.length > 0) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.company.list',
           params: {
               filter: { ID: entityIDs.COMPANY },
               select: ['ID', 'PHONE', 'EMAIL', 'TITLE']
           }
       });
       if (result.isSuccess) {
           const companies = result.getData()?.result;
           if (companies?.length > 0) {
               resultEntity.company = companies;
           }
       } else {
           console.error(result.getErrorMessages().join('; '));
       }
   }
   ```

- PHP

   ```php
   if(!empty($entityIDs['COMPANY']))
   {
       $companies = $sb->getCRMScope()->company()->list(
           [],
           ['ID' => $entityIDs['COMPANY']],
           ['ID', 'PHONE', 'EMAIL', 'TITLE']
       )->getCompanies();
       if(!empty($companies))
       {
           $resultEntity['company'] = $companies;
       }
   }
   ```

- Python

   ```python
   if entity_ids["COMPANY"]:
       result = client.crm.company.list(
           filter={"ID": entity_ids["COMPANY"]},
           select=["ID", "PHONE", "EMAIL", "TITLE"],
       ).result

       if result:
           result_entity["company"] = result
   ```

- Go

    ```go
    // A company has no first or last name — its name is in TITLE.
    res, err := core.Call(ctx, "crm.company.list", b24.Params{
    	"filter": b24.Params{"ID": entityIDs["COMPANY"]},
    	"select": []string{"ID", "TITLE", "PHONE", "EMAIL"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.company.list: %w", err)
    }
    var companies []entity
    if err := json.Unmarshal(res.Result, &companies); err != nil {
    	return fmt.Errorf("parse companies: %w", err)
    }
    for _, e := range companies {
    	rows = append(rows, e.row("company"))
    }
    ```

{% endlist %}

The method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) will return a list of companies based on the filter.

```
{
    "result":[{
        "ID":"587",
        "TITLE":"Romanka",
        "PHONE":[{
            "ID":"1899",
            "VALUE_TYPE":"WORK",
            "VALUE":"5345654",
            "TYPE_ID":"PHONE"
        }],
        "EMAIL":[{
            "ID":"1901",
            "VALUE_TYPE":"WORK",
            "VALUE":"company@xample.com",
            "TYPE_ID":"EMAIL"}]
        }]
}
```

## Display Results in a Table

Display the found records in the `Identifier`, Object type, `Name/First and last name`, `Phone`, `Email` columns.

{% list tabs %}

- JS

   ```js
   let table = [];
   
   table.push([
       "Identifier",
       "Object type",
       "Name/First and last name",
       "Phone",
       "Email"
   ]);
   
   for (let entity in resultEntity) {
       resultEntity[entity].forEach(function(item) {
           let phones = '';
           if (item.PHONE) {
               phones = item.PHONE.map(phone => phone.VALUE).join(', ');
           }
           let emails = '';
           if (item.EMAIL) {
               emails = item.EMAIL.map(email => email.VALUE).join(', ');
           }
           let title = item.TITLE ? item.TITLE + (item.NAME || item.LAST_NAME ? ': ' : '') : '';
           if (item.NAME || item.LAST_NAME) {
               title += [item.NAME, item.LAST_NAME].join(' ');
           }
   
           table.push([
               item.ID,
               entity,
               title,
               phones || '—',
               emails || '—'
           ]);
       });
   }
   
   console.table(table);
   ```

- PHP

   ```php
   $table = [];
   $table[] = [
       "Identifier",
       "Object type",
       "Name/First and last name",
       "Phone",
       "Email"
   ];
   
   foreach ($resultEntity as $entityType => $entities) {
       foreach ($entities as $item) {
           $phones = '';
           if (!empty($item->PHONE)) {
               $phones = implode(', ', array_map(fn($phone) => $phone->VALUE, $item->PHONE));
           }
           $emails = '';
           if (!empty($item->EMAIL)) {
               $emails = implode(', ', array_map(fn($email) => $email->VALUE, $item->EMAIL));
           }
           $title = !empty($item->TITLE) ? $item->TITLE : '';
           if (!empty($item->NAME) || !empty($item->LAST_NAME)) {
               $namePart = trim($item->NAME . ' ' . $item->LAST_NAME);
               if ($title) {
                   $title .= ': ' . $namePart;
               } else {
                   $title = $namePart;
               }
           }
   
           $table[] = [
               $item->ID,
               $entityType,
               $title ?: '—',
               $phones ?: '—',
               $emails ?: '—'
           ];
       }
   }
   
   foreach ($table as $row) {
       echo implode("\t", $row) . "\n";
   }
   ```

- Python

   ```python
   table = [[
       "Identifier",
       "Object type",
       "Name/First and last name",
       "Phone",
       "Email",
   ]]

   for entity_type, entities in result_entity.items():
       for item in entities:
           phones = ", ".join(phone["VALUE"] for phone in (item.get("PHONE") or []))
           emails = ", ".join(email["VALUE"] for email in (item.get("EMAIL") or []))
           title = item.get("TITLE") or ""
           name_part = " ".join(filter(None, [item.get("NAME"), item.get("LAST_NAME")]))
           if title and name_part:
               title = f"{title}: {name_part}"
           elif name_part:
               title = name_part

           table.append([
               item["ID"],
               entity_type,
               title or "—",
               phones or "—",
               emails or "—",
           ])

   for row in table:
       print("\t".join(map(str, row)))
   ```

- Go

    ```go
    if len(rows) == 0 {
    	fmt.Println("No duplicates found")
    	return nil
    }
    fmt.Println("ID\tType\tName\tPhone\tEmail")
    for _, r := range rows {
    	fmt.Printf("%d\t%s\t%s\t%s\t%s\n", r.ID, r.Kind, r.Title, r.Phone, r.Email)
    }
    ```

{% endlist %}

## Code Example

{% list tabs %}

- JS

   ```javascript
   import { createInterface } from 'node:readline/promises'
   import { B24Hook } from '@bitrix24/b24jssdk'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   // Requesting phone and email from the user
   const rl = createInterface({ input: process.stdin, output: process.stdout })
   const phone = await rl.question('Enter phone number: ')
   const email = await rl.question('Enter email: ')
   rl.close()

   // Initializing variables
   const entityIDs = {
       LEAD: [],
       CONTACT: [],
       COMPANY: []
   }

   const resultEntity = {
       lead: [],
       contact: [],
       company: []
   }

   // Merges identifiers from the method response with the entityIDs object
   function mergeDuplicates(data) {
       for (const type of ['LEAD', 'CONTACT', 'COMPANY']) {
           if (Array.isArray(data?.[type])) {
               entityIDs[type] = entityIDs[type].concat(data[type])
           }
       }
   }

   // Searching for duplicates by phone
   if (phone) {
       const phoneResult = await $b24.actions.v2.call.make({
           method: 'crm.duplicate.findbycomm',
           params: { type: 'PHONE', values: [phone] }
       })
       if (phoneResult.isSuccess) {
           mergeDuplicates(phoneResult.getData()?.result)
       } else {
           console.error('Error searching for duplicates by phone:', phoneResult.getErrorMessages().join('; '))
       }
   }

   // Searching for duplicates by email
   if (email) {
       const emailResult = await $b24.actions.v2.call.make({
           method: 'crm.duplicate.findbycomm',
           params: { type: 'EMAIL', values: [email] }
       })
       if (emailResult.isSuccess) {
           mergeDuplicates(emailResult.getData()?.result)
       } else {
           console.error('Error searching for duplicates by email:', emailResult.getErrorMessages().join('; '))
       }
   }

   // Processing leads
   if (entityIDs.LEAD.length > 0) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.lead.list',
           params: {
               filter: { ID: entityIDs.LEAD },
               select: ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE']
           }
       })
       if (result.isSuccess) {
           const leads = result.getData()?.result
           if (leads?.length > 0) {
               resultEntity.lead = leads
           }
       } else {
           console.error(result.getErrorMessages().join('; '))
       }
   }

   // Processing contacts
   if (entityIDs.CONTACT.length > 0) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.contact.list',
           params: {
               filter: { ID: entityIDs.CONTACT },
               select: ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL']
           }
       })
       if (result.isSuccess) {
           const contacts = result.getData()?.result
           if (contacts?.length > 0) {
               resultEntity.contact = contacts
           }
       } else {
           console.error(result.getErrorMessages().join('; '))
       }
   }

   // Processing companies
   if (entityIDs.COMPANY.length > 0) {
       const result = await $b24.actions.v2.call.make({
           method: 'crm.company.list',
           params: {
               filter: { ID: entityIDs.COMPANY },
               select: ['ID', 'PHONE', 'EMAIL', 'TITLE']
           }
       })
       if (result.isSuccess) {
           const companies = result.getData()?.result
           if (companies?.length > 0) {
               resultEntity.company = companies
           }
       } else {
           console.error(result.getErrorMessages().join('; '))
       }
   }

   // Forming the table
   const table = []
   // Table header
   table.push([
       "Identifier",
       "Object type",
       "Name/First and last name",
       "Phone",
       "Email"
   ])

   // Data rows
   for (const entity in resultEntity) {
       resultEntity[entity].forEach(function(item) {
           let phones = ''
           if (item.PHONE) {
               phones = item.PHONE.map(phone => phone.VALUE).join(', ')
           }
           let emails = ''
           if (item.EMAIL) {
               emails = item.EMAIL.map(email => email.VALUE).join(', ')
           }
           let title = item.TITLE ? item.TITLE + (item.NAME || item.LAST_NAME ? ': ' : '') : ''
           if (item.NAME || item.LAST_NAME) {
               title += [item.NAME, item.LAST_NAME].join(' ')
           }

           table.push([
               item.ID,
               entity,
               title,
               phones || '—',
               emails || '—'
           ])
       })
   }

   // Outputting the table to the console
   console.table(table)
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

   // Requesting phone and email from the user
   $phone = readline("Enter phone number: ");
   $email = readline("Enter email: ");

   // Initializing variables
   $entityIDs = [
       'LEAD' => [],
       'CONTACT' => [],
       'COMPANY' => []
   ];
   $resultEntity = [
       'lead' => [],
       'contact' => [],
       'company' => []
   ];

   // Merges identifiers from the method response with the $entityIDs array
   function mergeDuplicates(DuplicateResult $result, array &$entityIDs): void
   {
       $data = $result->getCoreResponse()->getResponseData()->getResult();
       foreach (['LEAD', 'CONTACT', 'COMPANY'] as $type) {
           if (!empty($data[$type]) && is_array($data[$type])) {
               $entityIDs[$type] = array_merge($entityIDs[$type], $data[$type]);
           }
       }
   }

   // Searching for duplicates by phone
   if($phone)
   {
       mergeDuplicates($sb->getCRMScope()->duplicate()->findByPhone([$phone]), $entityIDs);
   }

   // Searching for duplicates by email
   if($email)
   {
       mergeDuplicates($sb->getCRMScope()->duplicate()->findByEmail([$email]), $entityIDs);
   }

   // Processing leads
   if(!empty($entityIDs['LEAD']))
   {
       $leads = $sb->getCRMScope()->lead()->list(
           [],
           ['ID' => $entityIDs['LEAD']],
           ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE']
       )->getLeads();
       if(!empty($leads))
       {
           $resultEntity['lead'] = $leads;
       }
   }

   // Processing contacts
   if(!empty($entityIDs['CONTACT']))
   {
       $contacts = $sb->getCRMScope()->contact()->list(
           [],
           ['ID' => $entityIDs['CONTACT']],
           ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL'],
           0
       )->getContacts();
       if(!empty($contacts))
       {
           $resultEntity['contact'] = $contacts;
       }
   }

   // Processing companies
   if(!empty($entityIDs['COMPANY']))
   {
       $companies = $sb->getCRMScope()->company()->list(
           [],
           ['ID' => $entityIDs['COMPANY']],
           ['ID', 'PHONE', 'EMAIL', 'TITLE']
       )->getCompanies();
       if(!empty($companies))
       {
           $resultEntity['company'] = $companies;
       }
   }

   // Forming the table
   $table = [];

   // Table header
   $table[] = [
       "Identifier",
       "Object type",
       "Name/First and last name",
       "Phone",
       "Email"
   ];

   // Data rows
   foreach ($resultEntity as $entityType => $entities) {
       foreach ($entities as $item) {
           $phones = '';
           if (!empty($item->PHONE)) {
               $phones = implode(', ', array_map(fn($phone) => $phone->VALUE, $item->PHONE));
           }
           $emails = '';
           if (!empty($item->EMAIL)) {
               $emails = implode(', ', array_map(fn($email) => $email->VALUE, $item->EMAIL));
           }
           $title = !empty($item->TITLE) ? $item->TITLE : '';
           if (!empty($item->NAME) || !empty($item->LAST_NAME)) {
               $namePart = trim($item->NAME . ' ' . $item->LAST_NAME);
               if ($title) {
                   $title .= ': ' . $namePart;
               } else {
                   $title = $namePart;
               }
           }

           $table[] = [
               $item->ID,
               $entityType,
               $title ?: '—',
               $phones ?: '—',
               $emails ?: '—'
           ];
       }
   }

   // Outputting the table to the console using tabs
   foreach ($table as $row) {
       echo implode("\t", $row) . "\n";
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

    phone = input("Enter phone number: ")
    email = input("Enter email: ")

    entity_ids = {"LEAD": [], "CONTACT": [], "COMPANY": []}
    result_entity = {"lead": [], "contact": [], "company": []}

    try:
        if phone:
            phone_result = client.crm.duplicate.findbycomm(
                type="PHONE",
                values=[phone],
            ).response.result
            for key in entity_ids:
                entity_ids[key].extend(phone_result.get(key, []))

        if email:
            email_result = client.crm.duplicate.findbycomm(
                type="EMAIL",
                values=[email],
            ).response.result
            for key in entity_ids:
                entity_ids[key].extend(email_result.get(key, []))

        if entity_ids["LEAD"]:
            result = client.crm.lead.list(
                filter={"ID": entity_ids["LEAD"]},
                select=["ID", "NAME", "LAST_NAME", "PHONE", "EMAIL", "TITLE"],
            ).response.result
            if result:
                result_entity["lead"] = result

        if entity_ids["CONTACT"]:
            result = client.crm.contact.list(
                filter={"ID": entity_ids["CONTACT"]},
                select=["ID", "NAME", "LAST_NAME", "PHONE", "EMAIL"],
            ).response.result
            if result:
                result_entity["contact"] = result

        if entity_ids["COMPANY"]:
            result = client.crm.company.list(
                filter={"ID": entity_ids["COMPANY"]},
                select=["ID", "PHONE", "EMAIL", "TITLE"],
            ).response.result
            if result:
                result_entity["company"] = result
    except BitrixAPIError as error:
        print(error)

    table = [[
        "Identifier",
        "Object type",
        "Name/First and last name",
        "Phone",
        "Email",
    ]]

    for entity_type, entities in result_entity.items():
        for item in entities:
            phones = ""
            if item.get("PHONE"):
                phones = ", ".join(phone["VALUE"] for phone in item["PHONE"])

            emails = ""
            if item.get("EMAIL"):
                emails = ", ".join(email["VALUE"] for email in item["EMAIL"])

            title = item.get("TITLE") or ""
            if item.get("NAME") or item.get("LAST_NAME"):
                name_part = " ".join(filter(None, [item.get("NAME"), item.get("LAST_NAME")]))
                if title:
                    title = f"{title}: {name_part}"
                else:
                    title = name_part

            table.append([
                item["ID"],
                entity_type,
                title or "—",
                phones or "—",
                emails or "—",
            ])

    for row in table:
        print("\t".join(map(str, row)))
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates a lead, a contact, and a company with the same
    // phone number and email, finds them as duplicates, prints the table, and cleans up
    // itself. It runs on any portal, nothing needs to be edited.
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

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()
    	// The phone and the email to search by. Neighbouring tabs ask them from
    	// the user; here they are set by constants, because the example itself creates
    	// objects with these values.
    	const (
    		phone = "+49 900 555-35-35"
    		email = "duplicate@example.com"
    	)

    	// The IDs of the found objects and their detailed data. The keys are
    	// the same ones that crm.duplicate.findbycomm returns.
    	entityIDs := map[string][]b24.ID{"LEAD": nil, "CONTACT": nil, "COMPANY": nil}
    	rows := make([]row, 0)
    	// --- setup: our own duplicates

    	cleanup, err := createDuplicates(ctx, core, phone, email)
    	defer cleanup()
    	if err != nil {
    		return err
    	}

    	// --- step 1: search for duplicates by communications
    	// The method searches by ONE communication type per call, so the phone and the email are
    	// queried separately, and the IDs are accumulated in a shared map.
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

    		// The response is an object with the LEAD, CONTACT, and COMPANY keys. A key may be absent
    		// at all: if nothing was found for that type, it is simply not sent.
    		var found map[string][]b24.ID
    		if err := json.Unmarshal(res.Result, &found); err != nil {
    			return fmt.Errorf("parse duplicates: %w", err)
    		}
    		for key := range entityIDs {
    			entityIDs[key] = appendUnique(entityIDs[key], found[key])
    		}
    	}
    	fmt.Printf("found: leads %d, contacts %d, companies %d\n",
    		len(entityIDs["LEAD"]), len(entityIDs["CONTACT"]), len(entityIDs["COMPANY"]))

    	// --- step 2: the lead data

    	if len(entityIDs["LEAD"]) > 0 {
    		res, err := core.Call(ctx, "crm.lead.list", b24.Params{
    			"filter": b24.Params{"ID": entityIDs["LEAD"]},
    			"select": []string{"ID", "NAME", "LAST_NAME", "PHONE", "EMAIL", "TITLE"},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.lead.list: %w", err)
    		}
    		var leads []entity
    		if err := json.Unmarshal(res.Result, &leads); err != nil {
    			return fmt.Errorf("parse leads: %w", err)
    		}
    		for _, e := range leads {
    			rows = append(rows, e.row("lead"))
    		}
    	}

    	// --- step 3: the contact data

    	if len(entityIDs["CONTACT"]) > 0 {
    		res, err := core.Call(ctx, "crm.contact.list", b24.Params{
    			"filter": b24.Params{"ID": entityIDs["CONTACT"]},
    			"select": []string{"ID", "NAME", "LAST_NAME", "PHONE", "EMAIL"},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.contact.list: %w", err)
    		}
    		var contacts []entity
    		if err := json.Unmarshal(res.Result, &contacts); err != nil {
    			return fmt.Errorf("parse contacts: %w", err)
    		}
    		for _, e := range contacts {
    			rows = append(rows, e.row("contact"))
    		}
    	}

    	// --- step 4: the company data

    	if len(entityIDs["COMPANY"]) > 0 {
    		// A company has no first or last name — its name is in TITLE.
    		res, err := core.Call(ctx, "crm.company.list", b24.Params{
    			"filter": b24.Params{"ID": entityIDs["COMPANY"]},
    			"select": []string{"ID", "TITLE", "PHONE", "EMAIL"},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.company.list: %w", err)
    		}
    		var companies []entity
    		if err := json.Unmarshal(res.Result, &companies); err != nil {
    			return fmt.Errorf("parse companies: %w", err)
    		}
    		for _, e := range companies {
    			rows = append(rows, e.row("company"))
    		}
    	}

    	// --- print the table
    	if len(rows) == 0 {
    		fmt.Println("No duplicates found")
    		return nil
    	}
    	fmt.Println("ID\tType\tName\tPhone\tEmail")
    	for _, r := range rows {
    		fmt.Printf("%d\t%s\t%s\t%s\t%s\n", r.ID, r.Kind, r.Title, r.Phone, r.Email)
    	}
    	return nil
    }

    // entity is the common shape of a row in the lead, contact, and company lists: the field set
    // differs between them, but the ones needed here match.
    type entity struct {
    	ID       b24.ID       `json:"ID"`
    	Title    string       `json:"TITLE"`
    	Name     string       `json:"NAME"`
    	LastName string       `json:"LAST_NAME"`
    	Phone    []multifield `json:"PHONE"`
    	Email    []multifield `json:"EMAIL"`
    }

    // multifield is a row of a crm_multifield field: the phone and the email are stored
    // as a list of objects, even when there is a single value.
    type multifield struct {
    	Value string `json:"VALUE"`
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
    		Phone: first(e.Phone), Email: first(e.Email)}
    }

    func first(values []multifield) string {
    	if len(values) == 0 {
    		return "—"
    	}
    	return values[0].Value
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

    // --- helpers: data setup and cleanup

    // createDuplicates creates a lead, a contact, and a company with the same phone number and
    // and email — exactly the situation the scenario looks for. It returns a function
    // of the cleanup: it is called even when the setup broke off halfway.
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
    		{"crm.lead.add", "crm.lead.delete", b24.Params{"TITLE": "Filling out the CRM form \", "NAME": "Klaus", "LAST_NAME": "Weber"}},
    		{"crm.contact.add", "crm.contact.delete", b24.Params{"NAME": "Klaus", "LAST_NAME": "Weber"}},
    		{"crm.company.add", "crm.company.delete", b24.Params{"TITLE": "Filling out the CRM form \"}},
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
    			return cleanup, fmt.Errorf("parse the ID from %s: %w", spec.add, err)
    		}
    		created[spec.delete] = id
    	}
    	return cleanup, nil
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}
