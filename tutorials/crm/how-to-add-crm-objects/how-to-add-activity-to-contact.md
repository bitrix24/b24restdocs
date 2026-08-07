# Add Calendar Event for Client Management

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Calendar events can be added automatically to remind employees about meetings or calls with clients. An event linked to the client's contact will appear in the calendar of the responsible employee. A CRM activity will be created for the event in the contact's detail form.

To add an event to the calendar, we will sequentially execute two methods:

1. [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) — retrieve client data

2. [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) — create a calendar event

## 1. Retrieve Client Data

We will use the [crm.contact.get](../../../api-reference/crm/contacts/crm-contact-get.md) method with the client ID. For example, we are interested in the contact with ID `1`.

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

   ```javascript
   import { B24Hook } from '@bitrix24/b24jssdk'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   const response = await $b24.actions.v2.call.make({
       method: 'crm.contact.get',
       params: { id: 1 },
       requestId: 'contact-get'
   })
   ```

-  PHP

   ```php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Psr\Log\NullLogger;

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   $resultContact = $sb->getCRMScope()->contact()->get(1)->contact();
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

   response = client.crm.contact.get(
       bitrix_id=1,
   ).response
   ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.contact.get",
    	b24.Params{"id": contactID}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.get: %w", err)
    }

    // The phone and the responsible person are needed from the response. PHONE is a multifield: a list of
    // objects, even when there is a single number, and it arrives only if the contact
    // has any phone numbers at all.
    var contact struct {
    	ID           b24.ID `json:"ID"`
    	Name         string `json:"NAME"`
    	LastName     string `json:"LAST_NAME"`
    	AssignedByID b24.ID `json:"ASSIGNED_BY_ID"`
    	Phone        []struct {
    		ID        b24.ID `json:"ID"`
    		Value     string `json:"VALUE"`
    		ValueType string `json:"VALUE_TYPE"`
    	} `json:"PHONE"`
    }
    if err := json.Unmarshal(res.Result, &contact); err != nil {
    	return fmt.Errorf("parse contact: %w", err)
    }
    if len(contact.Phone) == 0 {
    	return fmt.Errorf("contact %d has no phone number", contactID)
    }
    ```

{% endlist %}

As a result, we will obtain client data, including the phone `PHONE` and the ID of the responsible employee `ASSIGNED_BY_ID`.

```json
{
    "result": {
        "ID": "1",
        "POST": "Managing Director",
        "COMMENTS": null ,
        "NAME": "Klaus",
        "SECOND_NAME": "Werner",
        "LAST_NAME": "Müller",
        "PHOTO": null,
        "LEAD_ID": null,
        "TYPE_ID": "SHARE",
        "SOURCE_ID": "SELF",
        "SOURCE_DESCRIPTION": null,
        "COMPANY_ID": "52",
        "BIRTHDATE": "",
        "EXPORT": "Y",
        "HAS_PHONE": "Y",
        "HAS_EMAIL": "Y",
        "HAS_IMOL": "N",
        "DATE_CREATE": "2023-08-18T12:43:42+03:00",
        "DATE_MODIFY": "2023-10-17T15:59:13+03:00",
        "ASSIGNED_BY_ID": "61",
        "CREATED_BY_ID": "57",
        "MODIFY_BY_ID": "47",
        "OPENED": "N",
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "ORIGIN_VERSION": null,
        "FACE_ID": null,
        "LAST_ACTIVITY_TIME": "2025-03-15T10:38:21+02:00",
        "ADDRESS": null,
        "ADDRESS_2": null,
        "ADDRESS_CITY": null,
        "ADDRESS_POSTAL_CODE": null,
        "ADDRESS_REGION": null,
        "ADDRESS_PROVINCE": null,
        "ADDRESS_COUNTRY": null,
        "ADDRESS_LOC_ADDR_ID": null,
        "UTM_SOURCE": null,
        "UTM_MEDIUM": null,
        "UTM_CAMPAIGN": null,
        "UTM_CONTENT": null,
        "UTM_TERM": null,
        "LAST_ACTIVITY_BY": "1",
        "PHONE": [
        {
            "ID": "1326",
            "VALUE_TYPE": "MOBILE",
            "VALUE": "498001001020",
            "TYPE_ID": "PHONE"
        },
        ],
        "EMAIL": [
        {
            "ID": "1328",
            "VALUE_TYPE": "WORK",
            "VALUE": "vronsky@example.com",
            "TYPE_ID": "EMAIL"
        },
        ]
    },
    "time": {
        "start": 1747737934.888428,
        "finish": 1747737934.945823,
        "duration": 0.057394981384277344,
        "processing": 0.029510021209716797,
        "date_start": "2025-05-20T13:45:34+03:00",
        "date_finish": "2025-05-20T13:45:34+03:00"
    }
}
```

## 2. Create Calendar Event

To create an event, we will use the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method. We need to pass the client data and arbitrary parameters for the new event.

- `SUBJECT` — event title. We will specify `calendar title`.

- `DESCRIPTION` — description. For example, `calendar body`.

- `DESCRIPTION_TYPE` — format of the description text. Possible values: `1` — plain text, `2` — HTML markup, `3` — BB code. We will set the value to `3`.

- `OWNER_ID` — contact ID. We will pass the client ID — `1`.

- `OWNER_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). Pass `3` — contact. A full list of object types can be retrieved using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method.

- `TYPE_ID` — the event type. We will specify `1` — meeting. A list of event types can be retrieved using the [crm.enum.activitytype](../../../api-reference/crm/auxiliary/enum/outdated/crm-enum-activity-type.md) method.

- `COMMUNICATIONS` — client contact details:

    - `VALUE` — the phone number; take the value `VALUE` from the `PHONE` array obtained in the first step,

    - `ENTITY_ID` — the customer identifier; pass `1`,

    - `ENTITY_TYPE_ID` — [object type ID](../../../api-reference/crm/data-types.md#object_type), we will pass `3` — contact.

- `START_TIME` and `END_TIME` — start and end date and time in [ISO 8601](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) format, we will specify, for example, a duration of one hour,

- `RESPONSIBLE_ID` — ID of the responsible person, we will pass `ASSIGNED_BY_ID`, which was obtained in the first step.

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'crm.activity.add',
        params: {
            fields: {
                "SUBJECT": "calendar title",
                "DESCRIPTION": "calendar body",
                "DESCRIPTION_TYPE": 3,
                "OWNER_ID": 1, 
                "OWNER_TYPE_ID": 3, 
                "TYPE_ID": 1, 
                "COMMUNICATIONS": [
                    {
                        'VALUE': "498001001020", 
                        'ENTITY_ID': 1, 
                        'ENTITY_TYPE_ID': 3
                    }
                ],
                "START_TIME": "2025-05-20T14:00:00",
                "END_TIME": "2025-05-20T15:00:00",
                "RESPONSIBLE_ID": 61 
            }
        },
        requestId: 'activity-add'
    });
    ```

-  PHP

    ```php
    $result = $sb->getCRMScope()->activity()->add(
        [
            "SUBJECT" => "calendar title",
            "DESCRIPTION" => "calendar body",
            "DESCRIPTION_TYPE" => 3,
            "OWNER_ID" => 1,
            "OWNER_TYPE_ID" => 3,
            "TYPE_ID" => 1,
            "COMMUNICATIONS" => [
                [
                    'VALUE' => "498001001020",
                    'ENTITY_ID' => 1,
                    'ENTITY_TYPE_ID' => 3
                ]
            ],
            "START_TIME" => "2025-05-20T14:00:00",
            "END_TIME" => "2025-05-20T15:00:00",
            "RESPONSIBLE_ID" => 61,
        ]
    );
    ```

- Python

    ```python
    response = client.crm.activity.add(
        fields={
            "SUBJECT": "calendar title",
            "DESCRIPTION": "calendar body",
            "DESCRIPTION_TYPE": 3,
            "OWNER_ID": 1,
            "OWNER_TYPE_ID": 3,
            "TYPE_ID": 1,
            "COMMUNICATIONS": [
                {
                    "VALUE": "498001001020",
                    "ENTITY_ID": 1,
                    "ENTITY_TYPE_ID": 3,
                }
            ],
            "START_TIME": "2025-05-20T14:00:00",
            "END_TIME": "2025-05-20T15:00:00",
            "RESPONSIBLE_ID": 61,
        }
    ).response
    ```

- Go

    ```go
    // The start and end times are in ISO 8601 format. Here it is a one-hour meeting,
    // tomorrow at the same time.
    start := time.Now().Add(24 * time.Hour)

    res, err = core.Call(ctx, "crm.activity.add", b24.Params{
    	"fields": b24.Params{
    		"SUBJECT":     "calendar title",
    		"DESCRIPTION": "calendar body",
    		// 1 — plain text, 2 — HTML, 3 — BB code.
    		"DESCRIPTION_TYPE": 3,
    		"OWNER_ID":         contact.ID,
    		"OWNER_TYPE_ID":    entityTypeContact,
    		// 1 — a meeting; crm.enum.activitytype returns the full list.
    		"TYPE_ID": 1,
    		// COMMUNICATIONS links the activity to the client's contact details:
    		// the value is taken from the PHONE multifield retrieved in step 1.
    		"COMMUNICATIONS": []b24.Params{{
    			"VALUE":          contact.Phone[0].Value,
    			"ENTITY_ID":      contact.ID,
    			"ENTITY_TYPE_ID": entityTypeContact,
    		}},
    		"START_TIME":     start.Format(time.RFC3339),
    		"END_TIME":       start.Add(time.Hour).Format(time.RFC3339),
    		"RESPONSIBLE_ID": contact.AssignedByID,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.add: %w", err)
    }

    // There is no wrapper: result is the ID of the created activity itself.
    var activityID b24.ID
    if err := json.Unmarshal(res.Result, &activityID); err != nil {
    	return fmt.Errorf("parse event ID: %w", err)
    }
    ```

{% endlist %}

If the event is created successfully, the method will return its ID. If you receive an `error`, refer to the documentation for the [crm.activity.add](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-add.md) method to understand possible errors.

```json
{
    "result": 6915,
}
```

## Code Example

The example creates an activity "Meeting" in the CRM contact detail form and an event lasting one hour in the employee's calendar.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function createCalendarActivity() {
        try {
            var contactID = 1;
            const responseContact = await $b24.actions.v2.call.make({
                method: 'crm.contact.get',
                params: { id: contactID },
                requestId: 'contact-get'
            });
            var resultContact = responseContact.getData().result;

            if (resultContact.ASSIGNED_BY_ID && resultContact.PHONE) {
                var contactPhone = resultContact.PHONE[0];
                var staffID = resultContact.ASSIGNED_BY_ID;
                await $b24.actions.v2.call.make({
                    method: 'crm.activity.add',
                    params: {
                        fields: {
                            "SUBJECT": "calendar title",
                            "DESCRIPTION": "calendar body",
                            "DESCRIPTION_TYPE": 3, // text type (crm.enum.contenttype): plain, HTML, BB-code
                            "OWNER_ID": contactID,
                            "OWNER_TYPE_ID": 3, // crm.enum.ownertype
                            "TYPE_ID": 1, // crm.enum.activitytype
                            "COMMUNICATIONS": [
                                {
                                    'VALUE': contactPhone.VALUE,
                                    'ENTITY_ID': contactID,
                                    'ENTITY_TYPE_ID': 3 // crm.enum.ownertype
                                }
                            ],
                            "START_TIME": new Date().toISOString(),
                            "END_TIME": new Date(new Date().getTime() + 3600 * 1000).toISOString(),
                            "RESPONSIBLE_ID": staffID,
                        }
                    },
                    requestId: 'activity-add'
                });
                console.log(JSON.stringify({ 'message': 'Activity add' }));
            } else {
                console.log(JSON.stringify({ 'message': 'Activity not added' }));
            }
        } catch (error) {
            console.error(error);
            console.log(JSON.stringify({ 'message': 'Activity not added: ' + error.message }));
        }
    }

    createCalendarActivity();
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

    $contactID = 1;
    try {
        $resultContact = $sb->getCRMScope()->contact()->get($contactID)->contact();
        $resultActivity = null;
        if (!empty($resultContact->ASSIGNED_BY_ID) && !empty($resultContact->PHONE))
        {
            $phones = $resultContact->PHONE;
            $contactPhone = reset($phones);
            $staffID = $resultContact->ASSIGNED_BY_ID;
            $resultActivity = $sb->getCRMScope()->activity()->add(
                [
                    "SUBJECT" => "calendar title",
                    "DESCRIPTION" => "calendar body",
                    "DESCRIPTION_TYPE" => 3,// text type (crm.enum.contenttype): plain, HTML, BB-code
                    "OWNER_ID" => $contactID,
                    "OWNER_TYPE_ID" => 3, // crm.enum.ownertype
                    "TYPE_ID" => 1, // crm.enum.activitytype
                    "COMMUNICATIONS" => [
                        [
                            'VALUE' => $contactPhone->VALUE,
                            'ENTITY_ID' => $contactID,
                            'ENTITY_TYPE_ID' => 3// crm.enum.ownertype
                        ]
                    ],
                    "START_TIME" => date("Y-m-d H:i:s", time()),
                    "END_TIME" => date("Y-m-d H:i:s", time() + 3600),
                    "RESPONSIBLE_ID" => $staffID,
                ]
            )->getId();
        }
        if (!empty($resultActivity))
        {
            echo json_encode(['message' => 'Activity add']);
        }
        else
        {
            echo json_encode(['message' => 'Activity not added']);
        }
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Activity not added: ' . $e->getMessage()]);
    }
    ```

- Python

    ```python
    from datetime import datetime, timedelta

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    contact_id = 1
    result_activity = None

    try:
        contact = client.crm.contact.get(bitrix_id=contact_id).response.result

        if contact.get("ASSIGNED_BY_ID") and contact.get("PHONE"):
            contact_phone = contact["PHONE"][0]
            staff_id = contact["ASSIGNED_BY_ID"]
            now = datetime.now()
            result_activity = client.crm.activity.add(
                fields={
                    "SUBJECT": "calendar title",
                    "DESCRIPTION": "calendar body",
                    "DESCRIPTION_TYPE": 3,
                    "OWNER_ID": contact_id,
                    "OWNER_TYPE_ID": 3,
                    "TYPE_ID": 1,
                    "COMMUNICATIONS": [
                        {
                            "VALUE": contact_phone["VALUE"],
                            "ENTITY_ID": contact_id,
                            "ENTITY_TYPE_ID": 3,
                        }
                    ],
                    "START_TIME": now.isoformat(timespec="seconds"),
                    "END_TIME": (now + timedelta(hours=1)).isoformat(timespec="seconds"),
                    "RESPONSIBLE_ID": staff_id,
                }
            ).response
    except BitrixAPIError as error:
        print({"message": f"Activity not added: {error}"})
    else:
        if result_activity and result_activity.result:
            print({"message": "Activity add"})
        else:
            print({"message": "Activity not added"})
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
    // The example is self-contained: it creates a contact with a phone number, reads its data,
    // creates a calendar event linked to this contact and cleans up after itself.
    // It runs on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"
    	"time"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // entityTypeContact is the ID of the "contact" object type from crm.enum.ownertype.
    const entityTypeContact = 3

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: our own contact with a phone number

    	contactID, err := addContact(ctx, core)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.contact.delete", b24.Params{"id": contactID})

    	// --- step 1: the client data
    	res, err := core.Call(ctx, "crm.contact.get",
    		b24.Params{"id": contactID}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.contact.get: %w", err)
    	}

    	// The phone and the responsible person are needed from the response. PHONE is a multifield: a list of
    	// objects, even when there is a single number, and it arrives only if the contact
    	// has any phone numbers at all.
    	var contact struct {
    		ID           b24.ID `json:"ID"`
    		Name         string `json:"NAME"`
    		LastName     string `json:"LAST_NAME"`
    		AssignedByID b24.ID `json:"ASSIGNED_BY_ID"`
    		Phone        []struct {
    			ID        b24.ID `json:"ID"`
    			Value     string `json:"VALUE"`
    			ValueType string `json:"VALUE_TYPE"`
    		} `json:"PHONE"`
    	}
    	if err := json.Unmarshal(res.Result, &contact); err != nil {
    		return fmt.Errorf("parse contact: %w", err)
    	}
    	if len(contact.Phone) == 0 {
    		return fmt.Errorf("contact %d has no phone number", contactID)
    	}
    	fmt.Printf("contact %d %s %s, phone %s, responsible %d\n",
    		contact.ID, contact.Name, contact.LastName, contact.Phone[0].Value, contact.AssignedByID)

    	// --- step 2: the calendar event
    	// The start and end times are in ISO 8601 format. Here it is a one-hour meeting,
    	// tomorrow at the same time.
    	start := time.Now().Add(24 * time.Hour)

    	res, err = core.Call(ctx, "crm.activity.add", b24.Params{
    		"fields": b24.Params{
    			"SUBJECT":     "calendar title",
    			"DESCRIPTION": "calendar body",
    			// 1 — plain text, 2 — HTML, 3 — BB code.
    			"DESCRIPTION_TYPE": 3,
    			"OWNER_ID":         contact.ID,
    			"OWNER_TYPE_ID":    entityTypeContact,
    			// 1 — a meeting; crm.enum.activitytype returns the full list.
    			"TYPE_ID": 1,
    			// COMMUNICATIONS links the activity to the client's contact details:
    			// the value is taken from the PHONE multifield retrieved in step 1.
    			"COMMUNICATIONS": []b24.Params{{
    				"VALUE":          contact.Phone[0].Value,
    				"ENTITY_ID":      contact.ID,
    				"ENTITY_TYPE_ID": entityTypeContact,
    			}},
    			"START_TIME":     start.Format(time.RFC3339),
    			"END_TIME":       start.Add(time.Hour).Format(time.RFC3339),
    			"RESPONSIBLE_ID": contact.AssignedByID,
    		},
    	})
    	if err != nil {
    		return fmt.Errorf("crm.activity.add: %w", err)
    	}

    	// There is no wrapper: result is the ID of the created activity itself.
    	var activityID b24.ID
    	if err := json.Unmarshal(res.Result, &activityID); err != nil {
    		return fmt.Errorf("parse event ID: %w", err)
    	}
    	defer del(ctx, core, "crm.activity.delete", b24.Params{"id": activityID})

    	fmt.Printf("event %d created for %s\n", activityID, start.Format("02.01.2006 15:04"))
    	return nil
    }

    // --- helpers: data setup and cleanup

    // addContact creates a contact with a phone number: the page takes a ready-made contact with
    // ID 1, but on someone else's portal that is a different person or nobody.
    func addContact(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.contact.add", b24.Params{
    		"fields": b24.Params{
    			"NAME":      "Klaus",
    			"LAST_NAME": "Müller",
    			// A multifield: a row without an ID ADDS a value. MultifieldAdd
    			// assembles it for you, so you do not get lost in the keys.
    			"PHONE": []map[string]any{
    				b24.MultifieldAdd("+49 800 100-10-20", "MOBILE"),
    			},
    		},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("crm.contact.add: %w", err)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(res.Result, &id)
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
