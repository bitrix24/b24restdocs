# How to Find Duplicates in CRM by Phone and Email

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: users with read access to CRM entities

You can automate the search for duplicates by phone and email address using a script. It will find leads, contacts, and companies with matching data, retrieve information about them, and display it in a table:

- object ID,

- object type: lead, contact, or company,

- title or first and last name,

- phone,

- email address.

To find duplicates, we will sequentially execute the following methods:

1. [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) — find duplicates by phone and email,

2. [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) — retrieve leads,

3. [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) — retrieve contacts,

4. [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) — retrieve companies.

## Prepare the Data

We will pass the phone number and email to the script using dialog boxes in the browser. The values will be stored in the variables `phone` and `email`.

If the data is entered correctly and duplicates are found, they will be displayed in the table. Otherwise, the table will be empty.

We will create arrays:

- `entityIDs` — IDs of found leads, contacts, companies,

- `$resultEntity` — detailed data about the found objects.

{% include [Note on examples](../../../_includes/examples.md) %}

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
   require_once('crest.php');
   
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

{% endlist %}

## 1. Find Duplicate Objects

To find duplicate objects by phone and email, we will call the method [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) twice. We will pass two parameters to it.

- `type` — type of communication, `PHONE` or `EMAIL`.

- `values` — array of phone numbers or email addresses. We will specify the variables `phone` and `email`.

The IDs of the found duplicates will be combined in the `entityIDs` array.

{% list tabs %}

- JS

   ```js
   if (phone) {
       BX24.callMethod(
           "crm.duplicate.findbycomm",
           {
               type: "PHONE",
               values: [phone]
           },
           function (phoneResult) {
               if (phoneResult.error()) {
                   console.error("Error finding duplicates by phone:", phoneResult.error());
               } else {
                   if (Array.isArray(phoneResult.data().LEAD)) {
                       entityIDs.LEAD = entityIDs.LEAD.concat(phoneResult.data().LEAD);
                   }
                   if (Array.isArray(phoneResult.data().CONTACT)) {
                       entityIDs.CONTACT = entityIDs.CONTACT.concat(phoneResult.data().CONTACT);
                   }
                   if (Array.isArray(phoneResult.data().COMPANY)) {
                       entityIDs.COMPANY = entityIDs.COMPANY.concat(phoneResult.data().COMPANY);
                   }
               }
           }
       );
    }
   
   if (email) {
       BX24.callMethod(
           "crm.duplicate.findbycomm",
           {
               type: "EMAIL",
               values: [email]
           },
           function (emailResult) {
               if (emailResult.error()) {
                   console.error("Error finding duplicates by email:", emailResult.error());
               } else {
                   if (Array.isArray(emailResult.data().LEAD)) {
                       entityIDs.LEAD = entityIDs.LEAD.concat(emailResult.data().LEAD);
                   }
                   if (Array.isArray(emailResult.data().CONTACT)) {
                       entityIDs.CONTACT = entityIDs.CONTACT.concat(emailResult.data().CONTACT);
                   }
                   if (Array.isArray(emailResult.data().COMPANY)) {
                       entityIDs.COMPANY = entityIDs.COMPANY.concat(emailResult.data().COMPANY);
                   }
               }
           }
       );
   }
   ```

- PHP

   ```php
   if($phone)
   {
       $result = CRest::call('crm.duplicate.findbycomm', [
           'type' => 'PHONE',
           'values' => [$phone]
       ]);
       if(is_array($result['result']['LEAD']))
       {
           $entityIDs['LEAD'] = array_merge($entityIDs['LEAD'], $result['result']['LEAD']);
       }
       if(is_array($result['result']['CONTACT']))
       {
           $entityIDs['CONTACT'] = array_merge($entityIDs['CONTACT'], $result['result']['CONTACT']);
       }
       if(is_array($result['result']['COMPANY']))
       {
           $entityIDs['COMPANY'] = array_merge($entityIDs['COMPANY'], $result['result']['COMPANY']);
       }
   }
   
   if($email)
   {
       $result = CRest::call('crm.duplicate.findbycomm', [
           'type' => 'EMAIL',
           'values' => [$email]
       ]);
       if(is_array($result['result']['LEAD']))
       {
           $entityIDs['LEAD'] = array_merge($entityIDs['LEAD'], $result['result']['LEAD']);
       }
       if(is_array($result['result']['CONTACT']))
       {
           $entityIDs['CONTACT'] = array_merge($entityIDs['CONTACT'], $result['result']['CONTACT']);
       }
       if(is_array($result['result']['COMPANY']))
       {
           $entityIDs['COMPANY'] = array_merge($entityIDs['COMPANY'], $result['result']['COMPANY']);
       }
   }
   ```

{% endlist %}

The method [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) will return lists of IDs of leads, contacts, and companies where the specified phone or email address is found.

## 2. Retrieve Leads

If the list of lead IDs is not empty, we will retrieve their data using the method [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md).

1. Apply a filter by ID.

2. Select fields: `ID`, `NAME`, `LAST_NAME`, `PHONE`, `EMAIL`, `TITLE`.

3. Save the result in the `resultEntity` array.

{% list tabs %}

- JS

   ```js
   if (entityIDs.LEAD.length > 0) {
       BX24.callMethod('crm.lead.list', {
           'filter': {
               'ID': entityIDs.LEAD
           },
           'select': ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE']
       }, function(result) {
           if (result.error()) {
               console.error(result.error());
           } else {
               if (result.data().length > 0) {
                   resultEntity.lead = result.data();
               }
           }
       });
   }
   ```

- PHP

   ```php
   if(!empty($entityIDs['LEAD']))
   {
       $result = CRest::call(
           'crm.lead.list',
           [
           'filter' => [
               'ID' => $entityIDs['LEAD']
           ],
           'select' =>     [
               'ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE'
           ]
           ]
       );
       if(!empty($result['result']))
       {
           $resultEntity['lead'] = $result['result'];
       }
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
        "TITLE":"Filling out the CRM form \"Contact Information Form for Open Channels\"",
        "PHONE":[{
            "ID":"1957",
            "VALUE_TYPE":"OTHER",
            "VALUE":"+13216464646",
            "TYPE_ID":"PHONE"
        }]
    }]
}
```

## 3. Retrieve Contacts

If the list of contact IDs is not empty, we will retrieve their data using the method [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md).

1. Apply a filter by ID.

2. Select fields: `ID`, `NAME`, `LAST_NAME`, `PHONE`, `EMAIL`.

3. Save the result in the `resultEntity` array.

{% list tabs %}

- JS

   ```js
   if (entityIDs.CONTACT.length > 0) {
       BX24.callMethod('crm.contact.list', {
           'filter': {
               'ID': entityIDs.CONTACT
           },
           'select': ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL']
       }, function(result) {
           if (result.error()) {
               console.error(result.error());
           } else {
               if (result.data().length > 0) {
                   resultEntity.contact = result.data();
               }
           }
       });
   }
   ```

- PHP

   ```php
   if(!empty($entityIDs['CONTACT']))
   {
       $result = CRest::call(
           'crm.contact.list',
           [
               'filter' => [
                   'ID' => $entityIDs['CONTACT']
               ],
               'select' =>     [
                   'ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL'
               ]
           ]
       );
       if(!empty($result['result']))
       {
           $resultEntity['contact'] = $result['result'];
       }
   }
   ```

{% endlist %}

The method [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) will return a list of contacts based on the filter.

```json
{
    "result":[{
        "ID":"23",
        "NAME":"Alexander",
        "LAST_NAME":"Alekseev",
        "EMAIL":[{
            "ID":"854",
            "VALUE_TYPE":"WORK",
            "VALUE":"alekseev@example.com",
            "TYPE_ID":"EMAIL"
        }]
    }]
}
```

## 4. Retrieve Companies

If the list of company IDs is not empty, we will retrieve their data using the method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md).

1. Apply a filter by ID.

2. Select fields: `ID`, `PHONE`, `EMAIL`, `TITLE`.

3. Save the result in the `resultEntity` array.

{% list tabs %}

- JS

   ```js
   if (entityIDs.COMPANY.length > 0) {
       BX24.callMethod('crm.company.list', {
           'filter': {
               'ID': entityIDs.COMPANY
           },
           'select': ['ID', 'PHONE', 'EMAIL', 'TITLE']
       }, function(result) {
           if (result.error()) {
               console.error(result.error());
           } else {
               if (result.data().length > 0) {
                   resultEntity.company = result.data();
               }
           }
       });
   }
   ```

- PHP

   ```php
   if(!empty($entityIDs['COMPANY']))
   {
       $result = CRest::call(
           'crm.company.list',
           [
               'filter' => [
                   'ID' => $entityIDs['COMPANY']
               ],
               'select' =>     [
                   'ID', 'PHONE', 'EMAIL', 'TITLE'
               ]
           ]
       );
       if(!empty($result['result']))
       {
           $resultEntity['company'] = $result['result'];
       }
   }
   ```

{% endlist %}

The method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) will return a list of companies based on the filter.

```
{
    "result":[{
        "ID":"587",
        "TITLE":"Daisy",
        "PHONE":[{
            "ID":"1899",
            "VALUE_TYPE":"WORK",
            "VALUE":"5345654",
            "TYPE_ID":"PHONE"
        }],
        "EMAIL":[{
            "ID":"1901",
            "VALUE_TYPE":"WORK",
            "VALUE":"company@example.com",
            "TYPE_ID":"EMAIL"}]
        }]
}
```

## Display Results in a Table

We will display the found records in the columns `Identifier`, `Object Type`, `Title/First and Last Name`, `Phone`, `Email`.

{% list tabs %}

- JS

   ```js
   let table = [];
   
   table.push([
       "Identifier",
       "Object Type",
       "Title/First and Last Name",
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
       "Object Type",
       "Title/First and Last Name",
       "Phone",
       "Email"
   ];
   
   foreach ($resultEntity as $entityType => $entities) {
       foreach ($entities as $item) {
           $phones = '';
           if (!empty($item['PHONE'])) {
               $phones = implode(', ', array_column($item['PHONE'], 'VALUE'));
           }
           $emails = '';
           if (!empty($item['EMAIL'])) {
               $emails = implode(', ', array_column($item['EMAIL'], 'VALUE'));
           }
           $title = !empty($item['TITLE']) ? $item['TITLE'] : '';
           if (!empty($item['NAME']) || !empty($item['LAST_NAME'])) {
               $namePart = trim($item['NAME'] . ' ' . $item['LAST_NAME']);
               if ($title) {
                   $title .= ': ' . $namePart;
               } else {
                   $title = $namePart;
               }
           }
   
           $table[] = [
               $item['ID'],
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

{% endlist %}

## Code Example

{% list tabs %}

- JS

   ```javascript
   // Requesting phone and email from the user
   let phone = prompt("Enter phone number:");
   let email = prompt("Enter email:");
   
   // Initializing variables
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
   
   // Searching for duplicates by phone 
   
    if (phone) {
       BX24.callMethod(
           "crm.duplicate.findbycomm",
           {
               type: "PHONE",
               values: [phone]
           },
           function (phoneResult) {
               if (phoneResult.error()) {
                   console.error("Error finding duplicates by phone:", phoneResult.error());
               } else {
                   if (Array.isArray(phoneResult.data().LEAD)) {
                       entityIDs.LEAD = entityIDs.LEAD.concat(phoneResult.data().LEAD);
                   }
                   if (Array.isArray(phoneResult.data().CONTACT)) {
                       entityIDs.CONTACT = entityIDs.CONTACT.concat(phoneResult.data().CONTACT);
                   }
                   if (Array.isArray(phoneResult.data().COMPANY)) {
                       entityIDs.COMPANY = entityIDs.COMPANY.concat(phoneResult.data().COMPANY);
                   }
               }
           }
       );
    }
   
   // Searching for duplicates by email 
   
   if (email) {
       BX24.callMethod(
           "crm.duplicate.findbycomm",
           {
               type: "EMAIL",
               values: [email]
           },
           function (emailResult) {
               if (emailResult.error()) {
                   console.error("Error finding duplicates by email:", emailResult.error());
               } else {
                   if (Array.isArray(emailResult.data().LEAD)) {
                       entityIDs.LEAD = entityIDs.LEAD.concat(emailResult.data().LEAD);
                   }
                   if (Array.isArray(emailResult.data().CONTACT)) {
                       entityIDs.CONTACT = entityIDs.CONTACT.concat(emailResult.data().CONTACT);
                   }
                   if (Array.isArray(emailResult.data().COMPANY)) {
                       entityIDs.COMPANY = entityIDs.COMPANY.concat(emailResult.data().COMPANY);
                   }
               }
           }
       );
   }
   
   setTimeout(function() {
       // Processing leads
       if (entityIDs.LEAD.length > 0) {
           BX24.callMethod('crm.lead.list', {
               'filter': {
                   'ID': entityIDs.LEAD
               },
               'select': ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE']
           }, 
           function(result) {
               if (result.error()) {
                   console.error(result.error());
               } else {
                   if (result.data().length > 0) {
                       resultEntity.lead = result.data();
                  }
               }
           });
       }
   
       // Processing contacts
       if (entityIDs.CONTACT.length > 0) {
           BX24.callMethod('crm.contact.list', {
               'filter': {
                   'ID': entityIDs.CONTACT
               },
               'select': ['ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL']
           }, function(result) {
               if (result.error()) {
                   console.error(result.error());
               } else {
                   if (result.data().length > 0) {
                       resultEntity.contact = result.data();
                   }
               }
           });
       }
   
       // Processing companies
       if (entityIDs.COMPANY.length > 0) {
           BX24.callMethod('crm.company.list', {
               'filter': {
                   'ID': entityIDs.COMPANY
               },
               'select': ['ID', 'PHONE', 'EMAIL', 'TITLE']
           }, function(result) {
               if (result.error()) {
                   console.error(result.error());
               } else {
                   if (result.data().length > 0) {
                       resultEntity.company = result.data();
                   }
               }
           });
       }
   
       setTimeout(function() {
           let table = [];
           // Table header
           table.push([
               "Identifier",
               "Object Type",
               "Title/First and Last Name",
               "Phone",
               "Email"
           ]);
   
           // Data rows
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
           
           // Output the table to the console
           console.table(table);
       }, 1000); // Delay for all requests to complete
   }, 1000);
   ```

- PHP

   ```php
   <?php
   require_once('crest.php');
   
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
   
   // Searching for duplicates by phone    
   if($phone)
   {
       $result = CRest::call('crm.duplicate.findbycomm', [
           'type' => 'PHONE',
           'values' => [$phone]
       ]);
       if(is_array($result['result']['LEAD']))
       {
           $entityIDs['LEAD'] = array_merge($entityIDs['LEAD'], $result['result']['LEAD']);
       }
       if(is_array($result['result']['CONTACT']))
       {
           $entityIDs['CONTACT'] = array_merge($entityIDs['CONTACT'], $result['result']['CONTACT']);
       }
       if(is_array($result['result']['COMPANY']))
       {
           $entityIDs['COMPANY'] = array_merge($entityIDs['COMPANY'], $result['result']['COMPANY']);
       }
   }
   
   // Searching for duplicates by email 
   if($email)
   {
       $result = CRest::call('crm.duplicate.findbycomm', [
           'type' => 'EMAIL',
           'values' => [$email]
       ]);
       if(is_array($result['result']['LEAD']))
       {
           $entityIDs['LEAD'] = array_merge($entityIDs['LEAD'], $result['result']['LEAD']);
       }
       if(is_array($result['result']['CONTACT']))
       {
           $entityIDs['CONTACT'] = array_merge($entityIDs['CONTACT'], $result['result']['CONTACT']);
       }
       if(is_array($result['result']['COMPANY']))
       {
           $entityIDs['COMPANY'] = array_merge($entityIDs['COMPANY'], $result['result']['COMPANY']);
       }
   }
   
   // Processing leads
   if(!empty($entityIDs['LEAD']))
   {
       $result = CRest::call(
           'crm.lead.list',
           [
           'filter' => [
               'ID' => $entityIDs['LEAD']
           ],
           'select' =>     [
               'ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL', 'TITLE'
           ]
           ]
       );
       if(!empty($result['result']))
       {
           $resultEntity['lead'] = $result['result'];
       }
   }
   
   // Processing contacts
   if(!empty($entityIDs['CONTACT']))
   {
       $result = CRest::call(
           'crm.contact.list',
           [
               'filter' => [
                   'ID' => $entityIDs['CONTACT']
               ],
               'select' =>     [
                   'ID', 'NAME', 'LAST_NAME', 'PHONE', 'EMAIL'
               ]
           ]
       );
       if(!empty($result['result']))
       {
           $resultEntity['contact'] = $result['result'];
       }
   }
   
   // Processing companies
   if(!empty($entityIDs['COMPANY']))
   {
       $result = CRest::call(
           'crm.company.list',
           [
               'filter' => [
                   'ID' => $entityIDs['COMPANY']
               ],
               'select' =>     [
                   'ID', 'PHONE', 'EMAIL', 'TITLE'
               ]
           ]
       );
       if(!empty($result['result']))
       {
           $resultEntity['company'] = $result['result'];
       }
   }
   
   // Forming the table
   $table = [];
   
   // Table header
   $table[] = [
       "Identifier",
       "Object Type",
       "Title/First and Last Name",
       "Phone",
       "Email"
   ];
   
   // Data rows
   foreach ($resultEntity as $entityType => $entities) {
       foreach ($entities as $item) {
           $phones = '';
           if (!empty($item['PHONE'])) {
               $phones = implode(', ', array_column($item['PHONE'], 'VALUE'));
           }
           $emails = '';
           if (!empty($item['EMAIL'])) {
               $emails = implode(', ', array_column($item['EMAIL'], 'VALUE'));
           }
           $title = !empty($item['TITLE']) ? $item['TITLE'] : '';
           if (!empty($item['NAME']) || !empty($item['LAST_NAME'])) {
               $namePart = trim($item['NAME'] . ' ' . $item['LAST_NAME']);
               if ($title) {
                   $title .= ': ' . $namePart;
               } else {
                   $title = $namePart;
               }
           }
   
           $table[] = [
               $item['ID'],
               $entityType,
               $title ?: '—',
               $phones ?: '—',
               $emails ?: '—'
           ];
       }
   }
   
   // Output the table to the console with tabulation
   foreach ($table as $row) {
       echo implode("\t", $row) . "\n";
   }
   
   ?>
   ```

{% endlist %}