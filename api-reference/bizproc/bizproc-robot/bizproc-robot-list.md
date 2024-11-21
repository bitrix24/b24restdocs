# Get a list of registered application robots bizproc.robot.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- required parameters not indicated
- no response in case of success
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of robots registered by the application.

## Examples

{% list tabs %}

- JS

	```javascript
	BX24.callMethod(
		'bizproc.robot.list',
		{},
		function(result)
		{
			if(result.error())
				alert("Error: " + result.error());
			else
				alert("Success: " + result.data().join(', '));
		}
	);
	```

- B24-PHP-SDK

	```php
	try {
		$result = $serviceBuilder
			->getBizProcScope()
			->robot()
			->list();

		foreach ($result->getRobots() as $robot) {
			print($robot->code);
			print($robot->name);
			print($robot->handlerUrl);
			print($robot->authUserId);
			print($robot->isUseSubscription ? 'Yes' : 'No');
			print($robot->isUsePlacement ? 'Yes' : 'No');
			if ($robot->createdDate instanceof DateTime) {
				print($robot->createdDate->format(DateTime::ATOM));
			}
		}
	} catch (Throwable $e) {
		// Handle the exception
		print('Error: ' . $e->getMessage());
	}
	```
{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}