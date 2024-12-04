# Register a new Automation rule bizproc.robot.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- No parameter types and links to the types page.
- No note about required parameters.
- Need to add separate tables and describe array parameters like PROPERTIES.
- A link to the article about robots with "waiting" is needed. Also, that article is necessary :)
- Examples are lacking.
- No standard blocks.

{% endnote %}

{% endif %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Edits are needed to meet the writing standard.
- Parameter types are not specified.
- No success response is provided.
- No error response is provided.

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method registers a new robot.

#|
|| **Parameter**         | **Description**  ||

|| **CODE^*^**         | Internal identifier of the robot. Allowed characters are `a-z`, `A-Z`, `0-9`, dot, dash, and underscore. Required parameter.   ||
|| **HANDLER^*^**        | Application URL to which data will be sent. Required parameter. ||
|| **AUTH_USER_ID^*^** | ID of the user whose token will be sent to the application. ||
|| **NAME^*^**         | Name of the robot. Can be a string or an associative array of localized strings. Required parameter. ||
|| **USE_SUBSCRIPTION** | Use of subscription. Allowed values are `Y` or `N`. Indicates whether the robot should wait for a response from the application. If the parameter is empty or not specified, the user can configure this parameter in the action settings in the workflow designer. ||
|| **PROPERTIES**     | Array of robot parameters. The list of values is similar to the values of the `RETURN_PROPERTIES` parameter. ||
|| **USE_PLACEMENT** | Allows opening additional robot settings in the application slider. Accepts values (`Y`/`N`). Optional parameter. ||
|| **PLACEMENT_HANDLER** | Widget code (handler on the application side). If the `USE_PLACEMENT` parameter is set to "Y" but `PLACEMENT_HANDLER` is not specified, an error occurs.   ||
|| **RETURN_PROPERTIES** | Array of returned values from the robot. This parameter controls the ability of the robot to wait for a response from the application and work with the data that comes in the response.

The system name of the parameter must start with a letter and can only contain characters `a-z`, `A-Z`, `0-9`, and underscore.

{% endnote %}

Each parameter must contain: 
 - **Name**,
 - **Description**,
 - **Type**, 
   - `bool` (Yes/No)
   - `date` (Date)
   - `datetime` (Date/Time)
   - `double` (Number)
   - `int` (Integer)
   - `select` (List) array of list values:
   - `string` (String)
   - `text` (Text)
   - `user` (User)
 - **Options** (only for **TYPE** equal to `select`)

```php
[
'value1' => 'title1',
'value2' => 'title2',
'value3' => 'title3',
'value4' => 'title4',
]
```

- **Required** (Y/N) - whether the parameter is required.
- **Multiple** (Y/N) - whether the parameter can have multiple values.
- **Default** - default value of the parameter. By default, the parameter type is `string`, optional, and non-multiple.||
|#

^*^ - required parameters

## Example

{% include [Note about examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

	```js
	var params = {
		'CODE': 'robot',
		'HANDLER': 'http:///robot.php',
		'AUTH_USER_ID': 1,
		'NAME': 'Robot Example',
		'PROPERTIES': {
			'bool': {
				'Name': 'Yes/No',
				'Type': 'bool',
				'Required': 'Y',
				'Multiple': 'N'
			},
			'date': {
				'Name': 'Date',
				'Type': 'date'
			},
			'datetime': {
				'Name': 'Date/Time',
				'Type': 'datetime'
			},
			'double': {
				'Name': 'Number',
				'Type': 'double',
				'Required': 'Y'
			},
			'int': {
				'Name': 'Integer',
				'Type': 'int'
			},
			'select': {
				'Name': 'List',
				'Type': 'select',
				'Options': {
					'one': 'one',
					'two': 'two'
				}
			},
			'string': {
				'Name': 'String',
				'Type': 'string',
				'Default': 'default string value'
			},
			'text': {
				'Name': 'Text',
				'Type': 'text'
			},
			'user': {
				'Name': 'User',
				'Type': 'user'
			}
		}
	};

	BX24.callMethod(
		'bizproc.robot.add',
		params,
		function(result)
		{
			if(result.error())
				alert("Error: " + result.error());
			else
				alert("Success: " + result.data());
		}
	);
	```

- PHP (B24PhpSdk)
  
	```php
	try {
		$result = $serviceBuilder
			->getBizProcScope()
			->robot()
			->add(
				'robot_code', // string $code
				'https://example.com/handler', // string $handlerUrl
				1, // int $b24AuthUserId
				['en' => 'Robot Name'], // array $localizedRobotName
				true, // bool $isUseSubscription
				[], // array $properties
				false, // bool $isUsePlacement
				[] // array $returnProperties
			);

		if ($result->isSuccess()) {
			print_r($result->getCoreResponse()->getResponseData()->getResult());
		} else {
			print("Failed to add robot.");
		}
	} catch (Throwable $e) {
		print("Error: " . $e->getMessage());
	}
	```
{% endlist %}