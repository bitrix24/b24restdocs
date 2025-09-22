# Working in the Context of the Current User

By default, the CRest library operates under the user who installed the application. However, there are situations where it is necessary to make a request to the Bitrix24 account under a user whose tokens were sent to the page via POST. For example, when a user opens the widget of your application. To achieve this, you can inherit the methods of the CRest class:

```php
require_once(__DIR__ . '/crest.php');
class CRestCurrent extends CRest
{
    protected static $dataExt = [];
    protected static function getSettingData()
    {
        $return = static::expandData(file_get_contents(__DIR__ . '/settings.json'));
        if(is_array($return))
        {
            if(!empty(static::$dataExt))
            {
                $return['access_token'] = htmlspecialchars(static::$dataExt['AUTH_ID']);
                $return['domain'] = htmlspecialchars(static::$dataExt['DOMAIN']);
                $return['refresh_token'] = htmlspecialchars(static::$dataExt['REFRESH_ID']);
                $return['application_token'] = htmlspecialchars(static::$dataExt['APP_SID']);
            }
            else
            {
                $return['access_token'] = htmlspecialchars($_REQUEST['AUTH_ID']);
                $return['domain'] = htmlspecialchars($_REQUEST['DOMAIN']);
                $return['refresh_token'] = htmlspecialchars($_REQUEST['REFRESH_ID']);
                $return['application_token'] = htmlspecialchars($_REQUEST['APP_SID']);
            }
        }
        return $return;
    }
    public static function setDataExt($data)
    {
        static::$dataExt = $data;
    }
}
```

After that, you can include the file with the new class in your code and use the methods for working with the current user when tokens are present on the page.

You can check the functionality by calling the method:

```php
$result = CRestCurrent::call('user.current');

echo '<pre>';
    print_r($result);
echo '</pre>';
```