# How to Retrieve a Customer Address from the CRM

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
>
> - [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md) — a user with permission to read contacts or companies
> - [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md) — a user with permission to read contacts, companies, and leads simultaneously
> - [crm.contact.userfield.list](../../../api-reference/crm/contacts/userfield/crm-contact-userfield-list.md) — an administrator
> - [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — a user with permission to read a contact

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A customer address is stored in Bitrix24 in two independent ways.

- In the [Company details](../../../api-reference/crm/requisites/index.md) of contacts and companies. This is the standard method. In the customer card, the address is displayed as a separate Company details field. A single customer may have multiple Company details, and within a single Company details entry, there can be several addresses of different types. Leads do not have Company details; their address is linked directly to the lead itself.
- In a custom field of type `address`. An administrator creates such a field separately for a specific CRM object type, and the value is stored as a string within the object itself.

These methods are not linked. An address from Company details does not populate a custom field, and an address from a custom field is not visible to `crm.address.*` methods. If it is unknown where a specific customer's address is filled in, check both methods.

This tutorial covers both. The primary scenario is retrieving the address from Company details, which consists of two steps.

1. Retrieve the customer's Company details identifiers using the [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md) method.
2. Retrieve the addresses for these Company details using the [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md) method.

The second method is described in the [Address From a Custom Field](#userfield) section.

## Prepare the Data

The following are required for the scenario:

- An incoming webhook with `crm` permission — the examples use it for authorization. Store the webhook URL in an environment variable rather than in the code.
- A customer identifier. The examples use a contact with `ID` `2429`. You can retrieve the identifier using the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) method with a filter on any known contact field, or for a company, using the [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) method. If only a phone number or Webmail is known, use the [“Search for Duplicates by Phone Number”](./search-by-phone-and-email.md) tutorial.

## 1. Retrieve Company Details Linked to a Contact

An address is not linked directly to a contact or company — it is linked to a Company details entry. Therefore, first retrieve the customer's Company details identifiers.

To do this, use the [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md) method with a filter:

- Specify the value `3` in `ENTITY_TYPE_ID` — the identifier for the [contact type](../../../api-reference/crm/data-types.md#object_type). For the company type, use the identifier `4`.
- Specify the contact identifier in `ENTITY_ID`; in the example, this is `2429`.

{% include [Note on examples](../../../_includes/examples.md) %}

The example steps follow one another. The SDK is initialized once here, and the existing instance is used thereafter.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.requisite.list',
        params: {
            filter: {
                ENTITY_TYPE_ID: 3,
                ENTITY_ID: 2429,
            },
            select: [
                'ID',
                'ENTITY_TYPE_ID',
                'ENTITY_ID',
            ],
        }
    });
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    $requisites = $sb->getCRMScope()->requisite()->list(
        [],
        [
            'ENTITY_TYPE_ID' => 3,
            'ENTITY_ID' => 2429,
        ],
        [
            'ID',
            'ENTITY_TYPE_ID',
            'ENTITY_ID',
        ]
    )->getRequisites();

    print_r($requisites);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain=os.environ["B24_DOMAIN"],
            webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
        )
    )
    # B24_DOMAIN = 'your-domain.bitrix24.com'
    # B24_WEBHOOK_TOKEN = 'user_id/webhook_key'

    result = client.crm.requisite.list(
        filter={
            "ENTITY_TYPE_ID": 3,
            "ENTITY_ID": 2429,
        },
        select=[
            "ID",
            "ENTITY_TYPE_ID",
            "ENTITY_ID",
        ],
    ).response.result
    ```

- Go

    ```go
    // The address is bound not to the contact but to its REQUISITE, so you first need
    // the requisite IDs.
    res, err := core.Call(ctx, "crm.requisite.list", b24.Params{
    	"filter": b24.Params{"ENTITY_TYPE_ID": typeContact, "ENTITY_ID": contactID},
    	"select": []string{"ID", "ENTITY_TYPE_ID", "ENTITY_ID"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.requisite.list: %w", err)
    }

    // Here the IDs arrive AS STRINGS ("361"), although crm.enum.* returns them
    // as numbers. b24.ID parses both spellings, a plain int does not.
    var requisites []struct {
    	ID b24.ID `json:"ID"`
    }
    if err := json.Unmarshal(res.Result, &requisites); err != nil {
    	return fmt.Errorf("parse requisites: %w", err)
    }
    if len(requisites) == 0 {
    	return fmt.Errorf("contact %d has no requisites, there is nowhere to store the address", contactID)
    }
    ```

{% endlist %}

The response will contain a list of the contact's Company details. In the example, there is one Company details entry, and its `ID` is `361`. This is the value required for the next request.

```json
{
    "result": [
        {
            "ID": "361",
            "ENTITY_TYPE_ID": "3",
            "ENTITY_ID": "2429"
        }
    ],
    "total": 1
}
```

If there are multiple Company details in the response, you must request addresses for each `ID` from `result`.

## 2. Retrieve Company Details Addresses

To retrieve addresses, use the [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md) method with the following filter:

- specify the value `8` in `ENTITY_TYPE_ID` — the identifier for the [company details type](../../../api-reference/crm/data-types.md#object_type)
- specify the company details identifier from step 1 in `ENTITY_ID`, which is `361` in the example

Without a type filter, the method returns all company details addresses. This allows you to obtain the complete list of customer addresses.

The lead address is retrieved using the same method, but without step 1. Specify `1` in `ENTITY_TYPE_ID` — the identifier for the lead type, and `ENTITY_ID` — the identifier of the lead itself.

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.address.list',
        params: {
            filter: {
                ENTITY_TYPE_ID: 8,
                ENTITY_ID: 361,
            },
        }
    });
    ```

- PHP

    ```php
    $addresses = $sb->getCRMScope()->address()->list(
        [],
        [
            'ENTITY_TYPE_ID' => 8,
            'ENTITY_ID' => 361,
        ],
        []
    )->getAddresses();

    print_r($addresses);
    ```

- Python

    ```python
    result = client.crm.address.list(
        filter={
            "ENTITY_TYPE_ID": 8,
            "ENTITY_ID": 361,
        }
    ).response.result
    ```

- Go

    ```go
    // Without a filter by type, the method returns all addresses of the requisite.
    res, err := core.Call(ctx, "crm.address.list", b24.Params{
    	"filter": b24.Params{"ENTITY_TYPE_ID": typeRequisite, "ENTITY_ID": r.ID},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.address.list: %w", err)
    }

    var addresses []address
    if err := json.Unmarshal(res.Result, &addresses); err != nil {
    	return fmt.Errorf("parse addresses: %w", err)
    }
    ```

{% endlist %}

The response contains all company details addresses. In the example, there are two — the street address and the delivery address.

```json
{
    "result": [
        {
            "TYPE_ID": "1",
            "ENTITY_TYPE_ID": "8",
            "ENTITY_ID": "361",
            "ADDRESS_1": "Tverskaya Street, 7",
            "ADDRESS_2": null,
            "CITY": "Berlin",
            "POSTAL_CODE": "125009",
            "REGION": null,
            "PROVINCE": "Berlin",
            "COUNTRY": "Germany",
            "COUNTRY_CODE": null,
            "LOC_ADDR_ID": "569",
            "ANCHOR_TYPE_ID": "3",
            "ANCHOR_ID": "2429"
        },
        {
            "TYPE_ID": "11",
            "ENTITY_TYPE_ID": "8",
            "ENTITY_ID": "361",
            "ADDRESS_1": "Granatny Lane, 10",
            "ADDRESS_2": null,
            "CITY": "Berlin",
            "POSTAL_CODE": "123001",
            "REGION": "Presnensky District",
            "PROVINCE": "Berlin",
            "COUNTRY": "Germany",
            "COUNTRY_CODE": null,
            "LOC_ADDR_ID": "571",
            "ANCHOR_TYPE_ID": "3",
            "ANCHOR_ID": "2429"
        }
    ],
    "total": 2
}
```

Key response fields:

- `TYPE_ID` — [address type](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md). In the example, `1` is the street address and `11` is the delivery address. The [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method returns the full list of types.
- `ADDRESS_1`, `ADDRESS_2`, `CITY`, `POSTAL_CODE`, `REGION`, `PROVINCE`, `COUNTRY` — the address components. You must construct the address string from these components, as the method does not provide a single field with a ready-made string. Unfilled components are returned as `null`, which is normal even for a completed address.
- `ANCHOR_TYPE_ID` and `ANCHOR_ID` — the type and identifier of the customer to whom the company details belong. In the example, `3` and `2429` refer to the original contact. Use this pair to verify that the address belongs to the correct customer.

To retrieve only one type of address, add `TYPE_ID` to the filter. For example, for a delivery address:

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.address.list',
        params: {
            filter: {
                ENTITY_TYPE_ID: 8,
                ENTITY_ID: 361,
                TYPE_ID: 11,
            },
        }
    });
    ```

- PHP

    ```php
    $addresses = $sb->getCRMScope()->address()->list(
        [],
        [
            'ENTITY_TYPE_ID' => 8,
            'ENTITY_ID' => 361,
            'TYPE_ID' => 11,
        ],
        []
    )->getAddresses();

    print_r($addresses);
    ```

- Python

    ```python
    result = client.crm.address.list(
        filter={
            "ENTITY_TYPE_ID": 8,
            "ENTITY_ID": 361,
            "TYPE_ID": 11,
        }
    ).response.result
    ```

- Go

    ```go
    res, err = core.Call(ctx, "crm.address.list", b24.Params{
    	"filter": b24.Params{
    		"ENTITY_TYPE_ID": typeRequisite,
    		"ENTITY_ID":      r.ID,
    		"TYPE_ID":        11, // 11 — delivery address
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.address.list by type: %w", err)
    }
    var delivery []address
    if err := json.Unmarshal(res.Result, &delivery); err != nil {
    	return fmt.Errorf("parse delivery addresses: %w", err)
    }
    ```

{% endlist %}

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Customer ID, which can be obtained using the crm.contact.list method
    const contactId = 2429;
    // CRM object type: 3 — contact, 4 — company
    const entityTypeId = 3;
    // Address type from crm.enum.addresstype, for example 11 — delivery address.
    // Leave null to get all customer addresses
    const addressTypeId = null;

    // Getting customer requisition IDs
    const requisiteResult = await $b24.actions.v2.call.make({
        method: 'crm.requisite.list',
        params: {
            filter: {
                ENTITY_TYPE_ID: entityTypeId,
                ENTITY_ID: contactId
            },
            select: ["ID"]
        }
    });

    if (!requisiteResult.isSuccess) {
        console.error(requisiteResult.getErrorMessages().join('; '));
    } else {
        const requisites = requisiteResult.getData().result;

        if (requisites.length === 0) {
            console.log("The customer has no requisitions, there is nowhere to store the address.");
        } else {
            const rows = [];

            // The customer may have several requisitions, we iterate through each one
            for (const requisite of requisites) {
                const filter = {
                    ENTITY_TYPE_ID: 8,
                    ENTITY_ID: requisite.ID
                };

                if (addressTypeId !== null) {
                    filter.TYPE_ID = addressTypeId;
                }

                const addressResult = await $b24.actions.v2.call.make({
                    method: 'crm.address.list',
                    params: { filter: filter }
                });

                if (!addressResult.isSuccess) {
                    console.error(addressResult.getErrorMessages().join('; '));
                    continue;
                }

                for (const address of addressResult.getData().result) {
                    rows.push({
                        "Requisition": requisite.ID,
                        "Address type": address.TYPE_ID,
                        "Address": address.ADDRESS_1 || "Not specified",
                        "City": address.CITY || "Not specified",
                        "Postal code": address.POSTAL_CODE || "Not specified",
                        "Country": address.COUNTRY || "Not specified"
                    });
                }
            }

            if (rows.length === 0) {
                console.log("Customer requisitions have no addresses.");
            } else {
                console.table(rows);
            }
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
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Customer ID, which can be obtained using the crm.contact.list method
    $contactId = 2429;
    // CRM object type: 3 — contact, 4 — company
    $entityTypeId = 3;
    // Address type from crm.enum.addresstype, for example 11 — delivery address.
    // Leave null to get all customer addresses
    $addressTypeId = null;

    try {
        // Getting customer requisition IDs
        $requisites = $sb->getCRMScope()->requisite()->list(
            [],
            [
                'ENTITY_TYPE_ID' => $entityTypeId,
                'ENTITY_ID' => $contactId
            ],
            ['ID']
        )->getRequisites();

        if (count($requisites) === 0) {
            echo 'The customer has no requisitions, there is nowhere to store the address.';
            return;
        }

        $rows = [];

        // The customer may have several requisitions, we iterate through each one
        foreach ($requisites as $requisite) {
            $filter = [
                'ENTITY_TYPE_ID' => 8,
                'ENTITY_ID' => $requisite->ID
            ];

            if ($addressTypeId !== null) {
                $filter['TYPE_ID'] = $addressTypeId;
            }

            $addresses = $sb->getCRMScope()->address()->list(
                [],
                $filter,
                []
            )->getAddresses();

            foreach ($addresses as $address) {
                $rows[] = [
                    'requisiteId' => $requisite->ID,
                    'typeId' => $address->TYPE_ID,
                    'address' => $address->ADDRESS_1 ?? 'Not specified',
                    'city' => $address->CITY ?? 'Not specified',
                    'postalCode' => $address->POSTAL_CODE ?? 'Not specified',
                    'country' => $address->COUNTRY ?? 'Not specified'
                ];
            }
        }

        if (count($rows) === 0) {
            echo 'Customer requisitions have no addresses.';
            return;
        }

        echo '<table border="1">';
        echo '<tr><th>Requisition</th><th>Address type</th><th>Address</th><th>City</th><th>Postal code</th><th>Country</th></tr>';
        foreach ($rows as $row) {
            echo '<tr>';
            foreach ($row as $value) {
                echo '<td>' . htmlspecialchars((string)$value) . '</td>';
            }
            echo '</tr>';
        }
        echo '</table>';
    } catch (\Throwable $e) {
        echo 'Error: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain=os.environ["B24_DOMAIN"],
            webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
        )
    )
    # B24_DOMAIN = 'your-domain.bitrix24.com'
    # B24_WEBHOOK_TOKEN = 'user_id/webhook_key'

    # Customer ID, which can be obtained using the crm.contact.list method
    contact_id = 2429
    # CRM object type: 3 — contact, 4 — company
    entity_type_id = 3
    # Address type from crm.enum.addresstype, for example 11 — delivery address.
    # Leave None to get all customer addresses
    address_type_id = None

    try:
        requisites = client.crm.requisite.list(
            filter={
                "ENTITY_TYPE_ID": entity_type_id,
                "ENTITY_ID": contact_id,
            },
            select=["ID"],
        ).response.result
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        if not requisites:
            print("The customer has no requisitions, there is nowhere to store the address.")
        else:
            rows = []

            # The customer may have several requisitions, we iterate through each one
            for requisite in requisites:
                address_filter = {
                    "ENTITY_TYPE_ID": 8,
                    "ENTITY_ID": requisite["ID"],
                }

                if address_type_id is not None:
                    address_filter["TYPE_ID"] = address_type_id

                try:
                    addresses = client.crm.address.list(
                        filter=address_filter,
                    ).response.result
                except BitrixAPIError as error:
                    print(f"Error: {error}")
                    continue

                for address in addresses:
                    rows.append(
                        [
                            str(requisite["ID"]),
                            str(address.get("TYPE_ID") or "Not specified"),
                            str(address.get("ADDRESS_1") or "Not specified"),
                            str(address.get("CITY") or "Not specified"),
                            str(address.get("POSTAL_CODE") or "Not specified"),
                            str(address.get("COUNTRY") or "Not specified"),
                        ]
                    )

            if not rows:
                print("Customer requisitions have no addresses.")
            else:
                print("Requisition\tAddress type\tAddress\tCity\tPostal code\tCountry")
                for row in rows:
                    print("\t".join(row))
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
    // The example is self-contained: it creates a contact with a requisite and two addresses,
    // finds the addresses the way the page describes and cleans up after itself. It runs
    // on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // The IDs of CRM object types: crm.enum.ownertype returns the full list.
    const (
    	typeContact   = 3 // a contact; a company is 4
    	typeRequisite = 8 // a requisite
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: a contact, its requisite, and two addresses

    	contactID, err := createClient(ctx, core)
    	if err != nil {
    		return err
    	}
    	// Deleting a contact also removes its requisites and the addresses of those requisites.
    	defer del(ctx, core, "crm.contact.delete", b24.Params{"id": contactID})

    	// --- step 1: the client requisites
    	// The address is bound not to the contact but to its REQUISITE, so you first need
    	// the requisite IDs.
    	res, err := core.Call(ctx, "crm.requisite.list", b24.Params{
    		"filter": b24.Params{"ENTITY_TYPE_ID": typeContact, "ENTITY_ID": contactID},
    		"select": []string{"ID", "ENTITY_TYPE_ID", "ENTITY_ID"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.requisite.list: %w", err)
    	}

    	// Here the IDs arrive AS STRINGS ("361"), although crm.enum.* returns them
    	// as numbers. b24.ID parses both spellings, a plain int does not.
    	var requisites []struct {
    		ID b24.ID `json:"ID"`
    	}
    	if err := json.Unmarshal(res.Result, &requisites); err != nil {
    		return fmt.Errorf("parse requisites: %w", err)
    	}
    	if len(requisites) == 0 {
    		return fmt.Errorf("contact %d has no requisites, there is nowhere to store the address", contactID)
    	}
    	fmt.Printf("requisites of contact %d: %d\n", contactID, len(requisites))

    	// --- step 2: the addresses of each requisite

    	for _, r := range requisites {
    		// Without a filter by type, the method returns all addresses of the requisite.
    		res, err := core.Call(ctx, "crm.address.list", b24.Params{
    			"filter": b24.Params{"ENTITY_TYPE_ID": typeRequisite, "ENTITY_ID": r.ID},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.address.list: %w", err)
    		}

    		var addresses []address
    		if err := json.Unmarshal(res.Result, &addresses); err != nil {
    			return fmt.Errorf("parse addresses: %w", err)
    		}
    		for _, a := range addresses {
    			fmt.Printf("  requisite %d, type %d: %s\n", r.ID, a.TypeID, a.String())
    		}

    		// To retrieve an address of only one type, add TYPE_ID to the filter.
    		res, err = core.Call(ctx, "crm.address.list", b24.Params{
    			"filter": b24.Params{
    				"ENTITY_TYPE_ID": typeRequisite,
    				"ENTITY_ID":      r.ID,
    				"TYPE_ID":        11, // 11 — delivery address
    			},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("crm.address.list by type: %w", err)
    		}
    		var delivery []address
    		if err := json.Unmarshal(res.Result, &delivery); err != nil {
    			return fmt.Errorf("parse delivery addresses: %w", err)
    		}
    		fmt.Printf("  of them delivery addresses: %d\n", len(delivery))
    	}

    	// --- the second storage option: a custom field of the address type

    	return userFieldAddresses(ctx, core, contactID)
    }

    // address is a single row of the crm.address.list response. The method has no ready-made address string
    // there is none: it is assembled from parts, and the unfilled parts arrive as null — this is
    // normal even for a filled-in address.
    type address struct {
    	TypeID     b24.ID `json:"TYPE_ID"`
    	Address1   string `json:"ADDRESS_1"`
    	City       string `json:"CITY"`
    	PostalCode string `json:"POSTAL_CODE"`
    	Country    string `json:"COUNTRY"`
    	// This pair is used to verify that the address belongs to the right client.
    	AnchorTypeID b24.ID `json:"ANCHOR_TYPE_ID"`
    	AnchorID     b24.ID `json:"ANCHOR_ID"`
    }

    func (a address) String() string {
    	out := ""
    	for _, part := range []string{a.PostalCode, a.Country, a.City, a.Address1} {
    		if part == "" {
    			continue
    		}
    		if out != "" {
    			out += ", "
    		}
    		out += part
    	}
    	if out == "" {
    		return "not specified"
    	}
    	return out
    }

    // userFieldAddresses reads the address stored not in the requisite but directly in
    // the contact — in a custom field of the address type. To the crm.address.* methods such an
    // the address is not visible, this is an independent storage option.
    func userFieldAddresses(ctx context.Context, core *b24.Core, contactID b24.ID) error {
    	res, err := core.Call(ctx, "crm.contact.userfield.list", b24.Params{
    		"filter": b24.Params{"USER_TYPE_ID": "address"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		// The method is available only to an administrator — this is no reason to abort the scenario
    		// with the addresses from the requisites, it has already run.
    		fmt.Fprintf(os.Stderr, "crm.contact.userfield.list: %v\n", err)
    		return nil
    	}
    	var fields []struct {
    		FieldName string `json:"FIELD_NAME"`
    		Multiple  string `json:"MULTIPLE"`
    	}
    	if err := json.Unmarshal(res.Result, &fields); err != nil {
    		return fmt.Errorf("parse custom fields: %w", err)
    	}

    	res, err = core.Call(ctx, "crm.contact.get",
    		b24.Params{"id": contactID}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.contact.get: %w", err)
    	}
    	for _, f := range fields {
    		// The shape of the value depends on MULTIPLE: for a multiple field it is an array.
    		raw, ok := b24.Unwrap(res.Result, f.FieldName)
    		if !ok || b24.IsEmpty(raw) {
    			fmt.Printf("  field %s (MULTIPLE=%s): not filled in\n", f.FieldName, f.Multiple)
    			continue
    		}
    		fmt.Printf("  field %s (MULTIPLE=%s): %s\n", f.FieldName, f.Multiple, raw)
    	}
    	if len(fields) == 0 {
    		fmt.Println("  contacts on this portal have no fields of the address type")
    	}
    	return nil
    }

    // --- helpers: data setup and cleanup

    // createClient creates a contact, its requisite, and two addresses in ONE linked
    // in a batch: 4 commands cost one call to the portal instead of four.
    func createClient(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	presetID, err := firstPreset(ctx, core)
    	if err != nil {
    		return 0, err
    	}

    	b := b24.NewBatch()
    	// Halt is MANDATORY for a linked batch. Without it, a failed producer is not
    	// an error for the server: it will substitute the placeholder text itself for the consumer
    	// as a value, and the requisite would be bound to a "contact" named "$result[...]".
    	b.Halt = true

    	if err := b.AddAs("contact", "crm.contact.add", b24.Params{
    		"fields": b24.Params{"NAME": "Klaus", "LAST_NAME": "Weber"},
    	}); err != nil {
    		return 0, err
    	}
    	// There is no path: crm.contact.add responds with a bare ID.
    	contactRef, err := b24.Ref("contact")
    	if err != nil {
    		return 0, err
    	}

    	if err := b.AddAs("requisite", "crm.requisite.add", b24.Params{
    		"fields": b24.Params{
    			"ENTITY_TYPE_ID": typeContact,
    			"ENTITY_ID":      contactRef,
    			"PRESET_ID":      presetID,
    			"NAME":           "Primary requisite",
    			"ACTIVE":         "Y",
    		},
    	}); err != nil {
    		return 0, err
    	}
    	requisiteRef, err := b24.Ref("requisite")
    	if err != nil {
    		return 0, err
    	}

    	// A single requisite can have several addresses, but no more than one
    	// of each type: a second crm.address.add with the same TYPE_ID will not go through.
    	// The address types are listed by crm.enum.addresstype: 1 — actual,
    	// 11 — a delivery address.
    	addresses := []struct {
    		cmd    b24.CmdID
    		fields b24.Params
    	}{
    		{"address_actual", b24.Params{"TYPE_ID": 1, "ADDRESS_1": "Tverskaya Street, 7",
    			"CITY": "Berlin", "POSTAL_CODE": "125009", "COUNTRY": "Germany"}},
    		{"address_delivery", b24.Params{"TYPE_ID": 11, "ADDRESS_1": "Granatny Lane, 10",
    			"CITY": "Berlin", "POSTAL_CODE": "123001", "COUNTRY": "Germany"}},
    	}
    	for _, a := range addresses {
    		a.fields["ENTITY_TYPE_ID"] = typeRequisite
    		a.fields["ENTITY_ID"] = requisiteRef
    		if err := b.AddAs(a.cmd, "crm.address.add", b24.Params{"fields": a.fields}); err != nil {
    			return 0, err
    		}
    	}

    	// The commands are executed in the order they were ADDED, whatever their names are —
    	// this is exactly what makes the chain work.
    	res, err := core.CallBatch(ctx, b)
    	if err != nil {
    		return 0, fmt.Errorf("prepare the data in a batch: %w", err)
    	}
    	raw, err := res.Get("contact")
    	if err != nil {
    		return 0, err
    	}
    	var contactID b24.ID
    	return contactID, json.Unmarshal(raw, &contactID)
    }

    func firstPreset(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.requisite.preset.list", b24.Params{
    		"select": []string{"ID", "NAME"}, "order": b24.Params{"ID": "ASC"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return 0, fmt.Errorf("crm.requisite.preset.list: %w", err)
    	}
    	var presets []struct {
    		ID b24.ID `json:"ID"`
    	}
    	if err := json.Unmarshal(res.Result, &presets); err != nil {
    		return 0, err
    	}
    	if len(presets) == 0 {
    		return 0, fmt.Errorf("the portal has no requisite templates")
    	}
    	return presets[0].ID, nil
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

## Address from Custom Field {#userfield}

If a Bitrix24 administrator has created a custom field of type `address` for a contact, the address is stored directly in the contact and is not visible via methods `crm.address.*`. Such an address is retrieved in two steps.

1. Find the field code using the [crm.contact.userfield.list](../../../api-reference/crm/contacts/userfield/crm-contact-userfield-list.md) method with the filter `USER_TYPE_ID` = `address`. This method is available only to an administrator.
2. Read the field value using the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method — it returns custom fields along with standard fields.

For a company, use [crm.company.userfield.list](../../../api-reference/crm/companies/userfields/crm-company-userfield-list.md) and [crm.company.get](../../../api-reference/crm/companies/crm-company-get.md); for a lead, use [crm.lead.userfield.list](../../../api-reference/crm/leads/userfield/crm-lead-userfield-list.md) and [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md).

{% list tabs %}

- JS

    ```javascript
    // 1. Searching for custom fields of type "address"
    const fieldsResult = await $b24.actions.v2.call.make({
        method: 'crm.contact.userfield.list',
        params: {
            filter: { USER_TYPE_ID: 'address' }
        }
    });

    if (!fieldsResult.isSuccess) {
        // Method is available only to the administrator
        console.error(fieldsResult.getErrorMessages().join('; '));
    } else {
        const addressFields = fieldsResult.getData().result;

        // 2. Reading the values of these fields for the contact
        const contactResult = await $b24.actions.v2.call.make({
            method: 'crm.contact.get',
            params: { id: 2429 }
        });

        if (!contactResult.isSuccess) {
            console.error(contactResult.getErrorMessages().join('; '));
        } else {
            const contact = contactResult.getData().result;

            for (const field of addressFields) {
                console.log(field.FIELD_NAME, contact[field.FIELD_NAME]);
            }
        }
    }
    ```

- PHP

    ```php
    // B24PhpSDK does not have a typed wrapper for crm.contact.userfield.list,
    // therefore we call the method via the SDK core
    try {
        $fields = $sb->core->call(
            'crm.contact.userfield.list',
            [
                'filter' => ['USER_TYPE_ID' => 'address']
            ]
        )->getResponseData()->getResult();

        $contact = $sb->core->call(
            'crm.contact.get',
            ['id' => 2429]
        )->getResponseData()->getResult();

        foreach ($fields as $field) {
            echo $field['FIELD_NAME'] . ': ' . print_r($contact[$field['FIELD_NAME']] ?? null, true) . PHP_EOL;
        }
    } catch (\Throwable $e) {
        // crm.contact.userfield.list is available only to the administrator
        echo 'Error: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    try:
        fields = client.crm.contact.userfield.list(
            filter={"USER_TYPE_ID": "address"},
        ).response.result

        contact = client.crm.contact.get(bitrix_id=2429).response.result
    except BitrixAPIError as error:
        # crm.contact.userfield.list is available only to the administrator
        print(f"Error: {error}")
    else:
        for field in fields:
            print(field["FIELD_NAME"], contact.get(field["FIELD_NAME"]))
    ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.contact.userfield.list", b24.Params{
    	"filter": b24.Params{"USER_TYPE_ID": "address"},
    }, b24.WithIdempotent())
    if err != nil {
    	// The method is available only to an administrator — this is no reason to abort the scenario
    	// with the addresses from the requisites, it has already run.
    	fmt.Fprintf(os.Stderr, "crm.contact.userfield.list: %v\n", err)
    	return nil
    }
    var fields []struct {
    	FieldName string `json:"FIELD_NAME"`
    	Multiple  string `json:"MULTIPLE"`
    }
    if err := json.Unmarshal(res.Result, &fields); err != nil {
    	return fmt.Errorf("parse custom fields: %w", err)
    }

    res, err = core.Call(ctx, "crm.contact.get",
    	b24.Params{"id": contactID}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.get: %w", err)
    }
    for _, f := range fields {
    	// The shape of the value depends on MULTIPLE: for a multiple field it is an array.
    	raw, ok := b24.Unwrap(res.Result, f.FieldName)
    	if !ok || b24.IsEmpty(raw) {
    		fmt.Printf("  field %s (MULTIPLE=%s): not filled in\n", f.FieldName, f.Multiple)
    		continue
    	}
    	fmt.Printf("  field %s (MULTIPLE=%s): %s\n", f.FieldName, f.Multiple, raw)
    }
    ```

{% endlist %}

In the response of the first request, we are interested in `FIELD_NAME` — the field code where the value is stored in the contact. Note `MULTIPLE`: the value format depends on it.

```json
{
    "result": [
        {
            "ID": "474",
            "ENTITY_ID": "CRM_CONTACT",
            "FIELD_NAME": "UF_CRM_1724412832",
            "USER_TYPE_ID": "address",
            "MULTIPLE": "N",
            "MANDATORY": "N"
        },
        {
            "ID": "475",
            "ENTITY_ID": "CRM_CONTACT",
            "FIELD_NAME": "UF_CRM_1724412960",
            "USER_TYPE_ID": "address",
            "MULTIPLE": "Y",
            "MANDATORY": "N"
        }
    ],
    "total": 2
}
```

The value of such a field is a string consisting of three parts separated by the `|` character: the text address, coordinates via `;`, and the address identifier in the `location` module. If `MULTIPLE` = `Y`, an array of such strings will be returned.

```json
{
    "result": {
        "ID": "2429",
        "UF_CRM_1724412832": "Granatny Lane, 10, Berlin, Berlin, Germany, 123001|55.761234;37.591234|571",
        "UF_CRM_1724412960": [
            "Tverskaya Street, 7, , Berlin, Berlin, Germany|;|575",
            ", , Berlin, Berlin, Germany|;|577"
        ]
    }
}
```

The text part is constructed from the same components as a company details address, so unfilled components result in consecutive commas. Coordinates can also be empty — in that case, a single separator `;` remains. Parse the string by `|` and do not assume that all three parts are filled.

## Verify the Result

The scenario is successful if:

- in the `crm.requisite.list` response, the `total` field is greater than zero and there is at least one `ID` in `result`
- in the `crm.address.list` response, the `total` field is greater than zero
- in every address, `ANCHOR_TYPE_ID` and `ANCHOR_ID` match the type and identifier of the original customer — in the example, `3` and `2429`

Do not consider `null` in individual address fields as an error: unfilled components arrive empty even for a correctly created address.

You can verify the data in the interface. Open the contact or company card and expand the "Company details" field. The addresses from the method response should match the addresses in the card's company details. The address from the custom field is displayed in the card as a separate field, not within the company details.

## Errors and Diagnostics

If the method returns an error or an empty result, check the request data.

- `Access denied.` in `crm.requisite.list` — the user does not have permission to read the object specified in `ENTITY_TYPE_ID`. Check the permissions to read contacts and companies in the CRM settings.
- `Access denied.` in `crm.requisite.list` with value `ENTITY_TYPE_ID` `1` — Company details only exist for contacts and companies; step 1 is skipped for a lead.
- `Access denied.` in `crm.address.list` — the method requires permissions to read contacts, companies, and leads simultaneously. The error will appear even if a specific detail can be read.
- `Access denied.` in `crm.contact.userfield.list` — the method was called by someone other than an administrator. This does not mean the address does not exist — the field code is retrieved as an administrator once, and thereafter `crm.contact.get` administrator is not required.
- `result` is empty in `crm.requisite.list` — the customer has no details; check the custom field. Also, ensure that `ENTITY_TYPE_ID` corresponds to the object from `ENTITY_ID`. With type `3` and a company identifier, the method will return an empty list without an error.
- `result` is empty in `crm.address.list` — the detail has no addresses, or addresses exist but of a different type. Repeat step 2 without the `TYPE_ID` filter and ensure that `8` is passed in `ENTITY_TYPE_ID`, and in `ENTITY_ID` the detail identifier from step 1 is passed, rather than the contact identifier.

## Key Considerations

- The address is linked to a detail, not directly to a contact or a company. A single customer may have multiple details, so iterate through the entire list from step 1 instead of just the first item.
- A single detail may contain multiple addresses of different types. Without the `TYPE_ID` filter, the method will return all of them.
- The method does not return a formatted address string. Construct it from the fields `ADDRESS_1`, `ADDRESS_2`, `CITY`, `POSTAL_CODE`, `REGION`, `PROVINCE`, and `COUNTRY`.
- The field `COUNTRY_CODE` is kept for backward compatibility and is not populated.
- The fields `ANCHOR_TYPE_ID` and `ANCHOR_ID` are service fields; they are populated automatically when an address is added.
- Linking an address directly to a contact or a company, bypassing the detail, is only possible in cases where the old address management mode was enabled by technical support. Do not rely on this link in new integrations.
- The two storage methods do not synchronize with each other. If an address is stored in a custom field, it will not appear in the details.

## Continue Learning

- [{#T}](../../../api-reference/crm/requisites/index.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-list.md)
- [{#T}](../../../api-reference/crm/requisites/universal/crm-requisite-list.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md)
- [{#T}](./search-by-phone-and-email.md)
