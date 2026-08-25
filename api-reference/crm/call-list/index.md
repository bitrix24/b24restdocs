# Call List: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A call list is a CRM activity from which multiple clients can be called consecutively. An employee works with a single activity that contains selected contacts or companies. The results of the calls will be automatically saved in the client cards:

- in the attached call activity and its comments
- in the filled CRM form and fields of the card
- in the deal or invoice created from the call list

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Calling in Bitrix24](https://helpdesk.bitrix24.com/open/21815426/)

## Getting Started

1. Retrieve the identifiers of the call list participants — contacts or companies — using the [crm.item.list](../universal/crm-item-list.md) method

2. Create a call list using the [crm.calllist.add](./crm-calllist-add.md) method: pass the object type in the `ENTITY_TYPE` parameter and the array of identifiers in the `ENTITIES` parameter. If necessary, attach a CRM form using the `WEBFORM_ID` parameter

3. Check the composition of the call list using the [crm.calllist.items.get](./crm-calllist-items-get.md) method, and the parameters of the call list itself using the [crm.calllist.get](./crm-calllist-get.md) method

4. The employee then calls the participants in the Bitrix24 interface. The statuses of the participants during the call list are returned by the same [crm.calllist.items.get](./crm-calllist-items-get.md) method, and the list of possible statuses is returned by the [crm.calllist.statuslist](./crm-calllist-statuslist.md) method

5. To add or remove participants of an already created call list, use the [crm.calllist.update](./crm-calllist-update.md) method

## Relationships with Other Objects

**Contact**. To call multiple contacts, specify an array of `ID` contacts in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of contacts using the [crm.item.list](../universal/crm-item-list.md) method with the parameter `entityTypeId = 3`.

**Company**. To call multiple companies, specify an array of `ID` companies in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of companies using the [crm.item.list](../universal/crm-item-list.md) method with the parameter `entityTypeId = 4`.

**CRM Form**. During the call, the employee can fill in information about the client in the CRM form attached to the call list. After the call is completed, the form will be available in the client card, and the information from the form will be saved in the fields of the card. To attach a form to the call list, specify its `ID` in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. The `ID` of the form can be found in the list of forms in Bitrix24 at `https://your-domain.com/crm/webform/`.

**Directory**. The status of the call in the call list card is an element of the [directory](../status/index.md). The list of statuses available to the call list participants is returned by the [crm.calllist.statuslist](./crm-calllist-statuslist.md) method. To change the statuses themselves, use the directory methods [crm.status.*](../status/index.md) with the parameter `ENTITY_ID = CALL_LIST`.

**User**. The call list is linked to its creator by a numeric identifier in the `CREATED_BY_ID` field. This field is returned by the [crm.calllist.get](./crm-calllist-get.md) and [crm.calllist.list](./crm-calllist-list.md) methods. You can obtain user data by identifier using the [user.get](../../user/user-get.md) method.

## How to Delete a Call List

There is no method for deleting a call list among the call list methods. You can delete a call list by deleting the activity using the [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md) method.

1. Use the [crm.activity.list](../timeline/activities/activity-base/crm-activity-list.md) method with a filter by the activity name `SUBJECT`. In the field value, specify `Call list #ID`. Replace `ID` with the identifier of the call list obtained from the [crm.calllist.add](./crm-calllist-add.md) or [crm.calllist.list](./crm-calllist-list.md) methods.

2. Pass the `ID` of the activity from the result to the deletion method [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md).

{% list tabs %}

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each activity item returned in result[]
    type ActivityItem = {
      ID: string,
    }

    try {
      // crm.activity.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const listResponse = await $b24.actions.v2.call.make<ActivityItem[]>({
        method: 'crm.activity.list',
        params: {
          filter: {
            SUBJECT: 'Call list #13',
          },
          select: ['ID'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!listResponse.isSuccess) {
        console.error(listResponse.getErrorMessages().join('; '))
      } else {
        const activities = listResponse.getData()!.result
        if (activities.length === 0) {
          console.info('No activities named "Call list #13" found.')
        } else {
          const activityId = activities[0].ID

          const deleteResponse = await $b24.actions.v2.call.make<boolean>({
            method: 'crm.activity.delete',
            params: {
              id: activityId,
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!deleteResponse.isSuccess) {
            console.error(deleteResponse.getErrorMessages().join('; '))
          } else {
            const deleted = deleteResponse.getData()!.result
            console.info('Activity ID', activityId, 'deleted:', deleted)
          }
        }
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function findAndDeleteActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.activity.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const listResponse = await $b24.actions.v2.call.make({
            method: 'crm.activity.list',
            params: {
              filter: {
                SUBJECT: 'Call list #13',
              },
              select: ['ID'],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!listResponse.isSuccess) {
            console.error(listResponse.getErrorMessages().join('; '))
            return
          }

          const activities = listResponse.getData().result
          if (activities.length === 0) {
            console.info('No activities named "Call list #13" found.')
            return
          }

          const activityId = activities[0].ID

          const deleteResponse = await $b24.actions.v2.call.make({
            method: 'crm.activity.delete',
            params: {
              id: activityId,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!deleteResponse.isSuccess) {
            console.error(deleteResponse.getErrorMessages().join('; '))
            return
          }

          const deleted = deleteResponse.getData().result
          console.info('Activity ID', activityId, 'deleted:', deleted)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', findAndDeleteActivity)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.list(
            filter={
                "SUBJECT": "Call list #13",
            },
            select=[
                "ID",
            ],
        ).response
        result = bitrix_response.result

        if not result:
            print("The activity titled 'Call list #13' was not found.")
        else:
            activity_id = int(result[0]["ID"])
            delete_response = client.crm.activity.delete(
                bitrix_id=activity_id,
            ).response
            delete_result = delete_response.result
            print(delete_result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.list',
                [
                    'filter' => [
                        'SUBJECT' => 'Call list #13',
                    ],
                    'select' => ['ID']
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            return;
        }
    
        $deals = $result->data();
    
        // If there are no activities, exit
        if (count($deals) === 0) {
            echo "No activities named 'Call list #13' were found.";
            return;
        }
    
        // Take the ID of the first activity
        $dealId = $deals[0]['ID'];
    
        // Call the delete method
        $deleteResponse = $b24Service
            ->core
            ->call(
                'crm.activity.delete',
                [
                    'id' => $dealId,
                ]
            );
    
        $deleteResult = $deleteResponse
            ->getResponseData()
            ->getResult();
    
        if ($deleteResult->error()) {
            error_log("Error deleting activity ID " . $dealId . ": " . $deleteResult->error());
        } else {
            echo "Activity ID " . $dealId . " was successfully deleted.";
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "crm.activity.list",
        {
            filter: {
                "SUBJECT": "Call list #13",
            },
            select: ["ID"]
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            let deals = result.data();

            // If there are no activities, exit
            if (deals.length === 0) {
                console.log("No activities named 'Call list #13' were found.");
                return;
            }

            // Take the ID of the first activity
            let dealId = deals[0].ID;

            // Call the delete method
            BX24.callMethod(
                'crm.activity.delete',
                {
                    id: dealId,
                },
                function(deleteResult) {
                    if (deleteResult.error()) {
                        console.error("Error deleting activity ID " + dealId + ": " + deleteResult.error());
                    } else {
                        console.log("Activity ID " + dealId + " was successfully deleted.");
                    }
                }
            );
        }
    );
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    // 1. Retrieve the list of activities by title
    $result = CRest::call(
        'crm.deal.list',
        [
            'filter' => [
                'TITLE' => 'Call list #13'
            ],
            'select' => ['ID']
        ]
    );

    if ($result['error']) {
        echo 'Error: ' . $result['error_description'];
        exit;
    }

    $deals = $result['result'];

    // If there are no activities, exit
    if (empty($deals)) {
        echo "No activities named 'Call list #13' were found.\n";
        exit;
    }

    // Take the ID of the first activity
    $dealId = $deals[0]['ID'];

    // 2. Delete the activity by ID
    $result = CRest::call(
        'crm.deal.delete',
        [
            'id' => $dealId
        ]
    );

    if ($result['error']) {
        echo "Error deleting activity ID $dealId: " . $result['error_description'] . "\n";
    } else {
        echo "Activity ID $dealId was successfully deleted.\n";
    }
    ```

{% endlist %}

## Access Permissions

Permissions depend on the operation:

- a user with read access to the specified contacts or companies can create and update a call list, as well as retrieve its participants: objects unavailable to the user will not be included in the call list, and the [crm.calllist.items.get](./crm-calllist-items-get.md) method will return only the available ones
- any user can retrieve the parameters of a call list and the list of call lists

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.calllist.add](./crm-calllist-add.md) | Creates a new call list ||
|| [crm.calllist.update](./crm-calllist-update.md) | Updates the composition of the call list ||
|| [crm.calllist.get](./crm-calllist-get.md) | Returns information about the call list ||
|| [crm.calllist.list](./crm-calllist-list.md) | Returns a list of all call lists ||
|| [crm.calllist.items.get](./crm-calllist-items-get.md) | Returns participants of the call list ||
|| [crm.calllist.statuslist](./crm-calllist-statuslist.md) | Returns a list of call list participant statuses ||
|#
