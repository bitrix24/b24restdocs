# How to Retrieve a Client's Address from CRM

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A client's address can be stored in Bitrix:

* in a custom field of the "address" type within any CRM object. To retrieve an address from a field, call the `get` or `list` method for the relevant object type.
* in the [Company details](../../../api-reference/crm/requisites/index.md) of contacts, companies, and leads. Within a single `Adresse` field, multiple addresses with their respective types may be stored. A single customer may have multiple sets of Company details recorded.

To retrieve a customer address from Company details, execute these two methods sequentially:

1. [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md)
2. [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md)

## 1. Retrieving Requisites Associated with a Contact

Obtaining the requisite ID is a necessary step, as the address is not directly linked to a contact or company. The address is associated with the requisite entity.

To retrieve the requisites, we use the crm.requisite.list method with the following filter:

* specify the value `3` in `ENTITY_TYPE_ID` — the identifier for the [contact type](../../../api-reference/crm/data-types.md#object_type). For the company type, use the identifier `4`.
* specify the contact ID in `ENTITY_ID`, which is `2429` in the example. You can retrieve the ID using the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) method with a filter on any known contact field. To retrieve a company ID, use [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md). If you need to retrieve a contact or company ID by phone number or email, use the [“Search for Duplicates by Phone Number”](./search-by-phone-and-email.md) tutorial.

{% include [Example Footnote](../../../_includes/examples.md) %}

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
                ENTITY_TYPE_ID: '3',
                ENTITY_ID: '2429',
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
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $requisites = $sb->getCRMScope()->requisite()->list(
        [],
        [
            'ENTITY_TYPE_ID' => '3',
            'ENTITY_ID' => '2429',
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
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.requisite.list(
        filter={
            "ENTITY_TYPE_ID": "3",
            "ENTITY_ID": "2429",
        },
        select=[
            "ID",
            "ENTITY_TYPE_ID",
            "ENTITY_ID",
        ],
    ).response.result
    ```

{% endlist %}

We obtained the requisite ID `361` — a parameter necessary for the next request.

 ```json
    Array
    (
     [result] => Array
        (
            [0] => Array
                (
                    [ID] => 361
                    [ENTITY_TYPE_ID] => 3
                    [ENTITY_ID] => 2429
                )
        )
     [total] => 1      
    )
 ```

## 2. Retrieving the Address

To obtain the address, we use the crm.address.list method with the following filter:

* specify the value `8` in `ENTITY_TYPE_ID` — the identifier for the [requisite type](../../../api-reference/crm/data-types.md#object_type).
* specify the requisite ID obtained in the previous request in `ENTITY_ID`, which is `361` in the example.
* specify the [Address type](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) in `TYPE_ID` if you need to retrieve a specific one. For example, the delivery address type is `11`, and the business address is `6`.

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

{% endlist %}

We received the delivery address data for the contact.

```json
    Array
    (
        [result] => Array
            (
                [0] => Array
                    (
                        [TYPE_ID] => 11
                        [ENTITY_TYPE_ID] => 8
                        [ENTITY_ID] => 361
                        [ADDRESS_1] => Granatengasse 10 c1
                        [ADDRESS_2] => 
                        [CITY] => Berlin
                        [POSTAL_CODE] => 123001
                        [REGION] => Bezirk Preschensky
                        [PROVINCE] => Berlin
                        [COUNTRY] => Deutschland
                        [COUNTRY_CODE] => 
                        [LOC_ADDR_ID] => 571
                        [ANCHOR_TYPE_ID] => 3
                        [ANCHOR_ID] => 2429
                    )
            )
        [total] => 1       
    )
 ```

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const contactId = "dein_kontakt_id_hier"; // Ersetze durch die tatsächliche Kontakt-ID

    // Methode zum Abrufen der Requisiten-ID
    const requisiteResult = await $b24.actions.v2.call.make({
        method: 'crm.requisite.list',
        params: {
            filter: {
                ENTITY_TYPE_ID: 3,
                ENTITY_ID: contactId
            },
            select: ["ID"]
        }
    });

    if (!requisiteResult.isSuccess) {
        console.error(requisiteResult.getErrorMessages().join('; '));
    } else {
        const requisites = requisiteResult.getData().result;
        if (requisites.length > 0) {
            const requisiteId = requisites[0].ID;
            console.log("Requisite ID:", requisiteId);

            // Methode zum Abrufen der Adresse
            const addressResult = await $b24.actions.v2.call.make({
                method: 'crm.address.list',
                params: {
                    filter: {
                        ENTITY_TYPE_ID: 8,
                        ENTITY_ID: requisiteId,
                        TYPE_ID: 11
                    }
                }
            });

            if (!addressResult.isSuccess) {
                console.error(addressResult.getErrorMessages().join('; '));
            } else {
                const addresses = addressResult.getData().result;
                if (addresses.length > 0) {
                    // Wir erstellen eine Tabelle zur Anzeige der Adressen
                    const table = [];
                    addresses.forEach(function(address) {
                        table.push({
                            "Adresse": address.ADDRESS_1 || "Nicht angegeben",
                            "Stadt": address.CITY || "Nicht angegeben",
                            "Postleitzahl": address.POSTAL_CODE || "Nicht angegeben",
                            "Land": address.COUNTRY || "Nicht angegeben"
                        });
                    });
                    console.table(table);
                } else {
                    console.log("Lieferadresse nicht gefunden.");
                }
            }
        } else {
            console.log("Requisite nicht gefunden.");
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
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $contactId = 'dein_kontakt_id_hier'; // Ersetze durch die tatsächliche Kontakt-ID

    try {
        // Methode zum Abrufen der Requisiten-ID
        $requisites = $sb->getCRMScope()->requisite()->list(
            [],
            [
                'ENTITY_TYPE_ID' => 3,
                'ENTITY_ID' => $contactId
            ],
            ['ID']
        )->getRequisites();

        if (count($requisites) > 0) {
            $requisiteId = $requisites[0]->ID;
            echo 'Requisite ID: ' . $requisiteId . PHP_EOL;

            // Methode zum Abrufen der Adresse
            $addresses = $sb->getCRMScope()->address()->list(
                [],
                [
                    'ENTITY_TYPE_ID' => 8,
                    'ENTITY_ID' => $requisiteId,
                    'TYPE_ID' => 11
                ],
                []
            )->getAddresses();

            if (count($addresses) > 0) {
                // Wir erstellen eine Tabelle zur Anzeige der Adressen
                echo '<table border="1">';
                echo '<tr><th>Adresse</th><th>Stadt</th><th>Postleitzahl</th><th>Land</th></tr>';
                foreach ($addresses as $address) {
                    echo '<tr>';
                    echo '<td>' . ($address->ADDRESS_1 ?? 'Nicht angegeben') . '</td>';
                    echo '<td>' . ($address->CITY ?? 'Nicht angegeben') . '</td>';
                    echo '<td>' . ($address->POSTAL_CODE ?? 'Nicht angegeben') . '</td>';
                    echo '<td>' . ($address->COUNTRY ?? 'Nicht angegeben') . '</td>';
                    echo '</tr>';
                }
                echo '</table>';
            } else {
                echo 'Lieferadresse nicht gefunden.';
            }
        } else {
            echo 'Requisite nicht gefunden.';
        }
    } catch (\Throwable $e) {
        echo 'Error: ' . $e->getMessage();
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

    contact_id = "dein_kontakt_id_hier"

    try:
        requisites = client.crm.requisite.list(
            filter={
                "ENTITY_TYPE_ID": 3,
                "ENTITY_ID": contact_id,
            },
            select=["ID"],
        ).response.result
    except BitrixAPIError as error:
        print(f"Fehler: {error}")
    else:
        if requisites:
            requisite_id = requisites[0]["ID"]
            print(f"Requisiten-ID: {requisite_id}")

            try:
                addresses = client.crm.address.list(
                    filter={
                        "ENTITY_TYPE_ID": 8,
                        "ENTITY_ID": requisite_id,
                        "TYPE_ID": 11,
                    }
                ).response.result
            except BitrixAPIError as error:
                print(f"Fehler: {error}")
            else:
                if addresses:
                    print("Adresse\tStadt\tPostleitzahl\tLand")
                    for address in addresses:
                        print(
                            "\t".join(
                                [
                                    str(address.get("ADDRESS_1") or "Nicht angegeben"),
                                    str(address.get("CITY") or "Nicht angegeben"),
                                    str(address.get("POSTAL_CODE") or "Nicht angegeben"),
                                    str(address.get("COUNTRY") or "Nicht angegeben"),
                                ]
                            )
                        )
                else:
                    print("Lieferadresse nicht gefunden.")
        else:
            print("Requisite nicht gefunden.")
    ```

{% endlist %}
