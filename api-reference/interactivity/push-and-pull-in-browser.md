# Push&Pull in the Browser

Let's look at how to work with the built-in Push & Pull client within the application. Here’s an example page for an application with a web interface:

```js
<!DOCTYPE html>
<html>
<head>
	<title>Bitri24 application with Push & Pull</title>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<script src="//api.bitrix24.com/api/v1/"></script>
	<script src="//api.bitrix24.com/api/v1/pull/"></script>
</head>
<body>
	<script>
		
		window.appPullClient = new BX.PullClient({
			restApplication: 'myApplication_test.bitrix24.com',
			restClient: BX24,
			userId: 1
		});
		window.appPullClient.subscribe({
			moduleId: 'application',
			callback: function (data) {
				console.warn(data); // {command: '...', params: {...}, extra: {...}}
			}.bind(this)
		});
		
		window.appPullClient.start();
				
	</script>
</body>
</html>
```

When initializing *BX.PullClient*, you need to specify the following parameters:

- `restApplication` — specify a custom string identifier for the application. (It must be unique for each account where your application is installed)
- `userId` — specify the identifier of the currently authorized user

If the web page is created by "1C-Bitrix: Site Management" and your edition includes the Push & Pull module, it is recommended to connect the built-in library instead of:

```js
<script src="//api.bitrix24.com/api/v1/pull/"></script> 
```

by using:

```js
\Bitrix\Main\UI\Extension::load("pull.client");
```

The minimum version of the Push & Pull module included is 18.5.500.

Connecting the PullClient will allow the front-end of your application to receive events from the channel that will be sent there by the back-end of your application using the method [pull.application.event.add](./push-and-pull/pull-application-event-add.md).