# Update Fields of Action bizproc.activity.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to pages not yet created are not specified.
  
{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method allows updating the fields of an already added action for workflows. The method parameters are similar to [bizproc.activity.add](./bizproc-activity-add.md).

## Example

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":24,"vatId":0,"productIblockId":23,"skuPropertyId":97}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":24,"vatId":0,"productIblockId":23,"skuPropertyId":97},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.add
    ```

- JS

    ```javascript
    function updateActivity1()
    {
        var params = {
            'CODE': 'hash',
            'FIELDS': {
                'DOCUMENT_TYPE': '',
                'FILTER': ''
            },
        };

        BX24.callMethod(
            'bizproc.activity.update',
            params,
            function(result)
            {
                if(result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $result = $serviceBuilder
            ->getBizProcScope()
            ->activity()
            ->update(
                'activity_code',
                'https://example.com/handler',
                1,
                ['en' => 'Activity Name', 'de' => 'Aktivitätsname'],
                ['en' => 'Activity Description', 'de' => 'Aktivitätsbeschreibung'],
                true,
                ['param1' => 'value1'],
                false,
                ['returnParam1' => 'value1'],
                null,
                null
            );

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print('Update failed.');
        }
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- PHP (CRest)

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.catalog.add',
        [
            'fields' => [
                'iblockId' => 24,
                'vatId' => 0,
                'productIblockId' => 23,
                'skuPropertyId' => 97,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}