# Call List: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A call list is a set of contacts or companies that an employee calls one after another. For example, the employee invites all clients from one city to a webinar.

In CRM, a call list is shown as an activity — a scheduled action, like a call, meeting, or email. The employee makes the calls from this activity. The results of the calls are automatically saved in the client cards:

- in the attached call activity and its comments
- in the filled CRM form and fields of the card
- in the deal or invoice created from the call list

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Calling in Bitrix24](https://helpdesk.bitrix24.com/open/21815426/)

## Getting Started

1. Retrieve the identifiers of the call list participants — contacts or companies — using the [crm.item.list](../universal/crm-item-list.md) method

2. Create a call list using the [crm.calllist.add](./crm-calllist-add.md) method: pass the object type in the `ENTITY_TYPE` parameter and the array of identifiers in the `ENTITIES` parameter. To let the employee fill in the client's data during a call, attach a CRM form using the `WEBFORM_ID` parameter

3. Check the composition of the call list using the [crm.calllist.items.get](./crm-calllist-items-get.md) method, and the parameters of the call list itself using the [crm.calllist.get](./crm-calllist-get.md) method

4. The employee then calls the participants in the Bitrix24 interface. You can track the progress of the call list using the same [crm.calllist.items.get](./crm-calllist-items-get.md) method: it returns the call status for each participant. A participant in the response looks like this:

    ```json
    {
        "ID": 9,
        "STATUS": "IN_WORK",
        "ENTITY_TYPE": 3
    }
    ```

    `ID` — identifier of the contact or company. `ENTITY_TYPE` — participant type: `3` — contact, `4` — company. `STATUS` — call status code. All codes are returned by the [crm.calllist.statuslist](./crm-calllist-statuslist.md) method

5. Change the participants using the [crm.calllist.update](./crm-calllist-update.md) method. The method replaces the entire list: pass everyone who must remain together with the new participants. If a CRM form is attached to the call list, pass `WEBFORM_ID` as well — without it, the method detaches the form

## Relationships with Other Objects

**Contact and Company.** The call list participants are set by an array of contact or company `ID`s in the call list [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method. You can retrieve the `ID`s using the [crm.item.list](../universal/crm-item-list.md) method: for contacts, with the parameter `entityTypeId = 3`, for companies, with `entityTypeId = 4`.

**CRM Form.** You can attach a CRM form to a call list: during a call, the employee can fill in the client's data in it. After the call, the form appears in the client card, and its data is saved in the card fields. To attach a form, specify its `ID` in the call list [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method. The form `ID` is returned by the [crm.form.list](../form/crm-form-list.md) method, and you can also see it in the list of forms in Bitrix24 at `https://your-domain.com/crm/webform/`.

**Directory.** Call statuses, for example `IN_WORK`, are stored in the [directory](../status/index.md) with `ENTITY_ID = CALL_LIST`, which is handled by the [crm.status.*](../status/index.md) methods. All statuses are returned by the [crm.calllist.statuslist](./crm-calllist-statuslist.md) method, and you can change a status by its `ID` using the [crm.status.update](../status/crm-status-update.md) method.

**User.** The `CREATED_BY_ID` field in the response of the [crm.calllist.get](./crm-calllist-get.md) and [crm.calllist.list](./crm-calllist-list.md) methods contains the numeric identifier of the user who created the call list. The data of this user is returned by the [user.get](../../user/user-get.md) method.

## How to Remove a Call List from the Timeline

A call list has no delete method. You can remove a call list from the timeline by deleting its activity using the [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md) method. The call list itself remains: the [crm.calllist.get](./crm-calllist-get.md), [crm.calllist.list](./crm-calllist-list.md), and [crm.calllist.items.get](./crm-calllist-items-get.md) methods continue to return it. After the activity is deleted, the user responsible for the activity loses access to the call list unless they are the creator of the call list or an administrator.

1. Find the call list activity using the [crm.activity.list](../timeline/activities/activity-base/crm-activity-list.md) method with the filter `PROVIDER_ID` = `CALL_LIST` and `ASSOCIATED_ENTITY_ID` = the call list identifier. The call list identifier is returned by the [crm.calllist.add](./crm-calllist-add.md) and [crm.calllist.list](./crm-calllist-list.md) methods.

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
            PROVIDER_ID: 'CALL_LIST',
            ASSOCIATED_ENTITY_ID: 13,
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
          console.info('No activity found for call list 13.')
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
                PROVIDER_ID: 'CALL_LIST',
                ASSOCIATED_ENTITY_ID: 13,
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
            console.info('No activity found for call list 13.')
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
                "PROVIDER_ID": "CALL_LIST",
                "ASSOCIATED_ENTITY_ID": 13,
            },
            select=[
                "ID",
            ],
        ).response
        result = bitrix_response.result

        if not result:
            print("No activity found for call list 13.")
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
                        'PROVIDER_ID' => 'CALL_LIST',
                        'ASSOCIATED_ENTITY_ID' => 13,
                    ],
                    'select' => ['ID']
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        $deals = $result;

        // If there are no activities, exit
        if (count($deals) === 0) {
            echo "No activity found for call list 13.";
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

        echo "Activity ID " . $dealId . " was successfully deleted.";
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "crm.activity.list",
        {
            filter: {
                "PROVIDER_ID": "CALL_LIST",
                "ASSOCIATED_ENTITY_ID": 13,
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
                console.log("No activity found for call list 13.");
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

    // 1. Find the call list activity by the call list identifier
    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'PROVIDER_ID' => 'CALL_LIST',
                'ASSOCIATED_ENTITY_ID' => 13
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
        echo "No activity found for call list 13.\n";
        exit;
    }

    // Take the ID of the first activity
    $dealId = $deals[0]['ID'];

    // 2. Delete the activity by ID
    $result = CRest::call(
        'crm.activity.delete',
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

- any user can create a call list: contacts or companies that the user cannot read are not included in it
- an administrator, the creator of the call list, or the user responsible for the call list activity can update a call list and retrieve its parameters. Read permission for contacts or companies is also required, depending on the type of call list participants
- an administrator, the creator of the call list, or the user responsible for the call list activity can retrieve the participants. The [crm.calllist.items.get](./crm-calllist-items-get.md) method returns only those the user can read
- any user can retrieve the list of call lists: an administrator receives all call lists; other users receive only the ones they created

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.calllist.add](./crm-calllist-add.md) | Creates a new call list ||
|| [crm.calllist.update](./crm-calllist-update.md) | Changes the participants and the CRM form of the call list ||
|| [crm.calllist.get](./crm-calllist-get.md) | Returns the call list parameters without participants ||
|| [crm.calllist.list](./crm-calllist-list.md) | Returns a list of call lists ||
|| [crm.calllist.items.get](./crm-calllist-items-get.md) | Returns participants of the call list ||
|| [crm.calllist.statuslist](./crm-calllist-statuslist.md) | Returns the possible call statuses in the call list ||
|#
