# RestClient

> Use the RestClient globally

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

$restClient = \Shoprunback\RestClient::getClient();
$restClient->useSandboxEnvironment();
$restClient->setToken('yourApiToken');

define('SRB_REST_CLIENT', $restClient);
```

**Accepts: GET ; POST ; PUT ; DELETE**

The RestClient is used to execute the API calls.

It is a **Singleton**, so you need to use `getClient` to use it.

<aside class="warning">
  Since it is a Singleton, it is highly recommended to <b>declare it in a variable or a constant</b> so you can use it everywhere
</aside>

## Set token

> Set token

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

$restClient = \Shoprunback\RestClient::getClient();
$restClient->setToken('yourApiToken');
```

You can **set the token of a user** with `setToken()`. Most of the API calls need you to be authentified to be done.

## Use production or sandbox environment

> Set environment

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

$restClient = \Shoprunback\RestClient::getClient();

if (/* Are we on production environment? */) {
  $restClient->useProductionEnvironment();
} else {
  $restClient->useSandboxEnvironment();
}
```

If you want to **use the production or sandbox environment**, you can use `useProductionEnvironment` and `useSandboxEnvironment`.

It will set **https://dashboard.shoprunback.com** for the production environment and **https://sandbox.dashboard.shoprunback.com** for the sandbox environment.