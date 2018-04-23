### Brand

> Initialize

```php
<?php
// With a use statement
require 'path/to/lib/shoprunback-php/init.php';

use \Shoprunback\Elements\Brand;

$brand = new Brand();

// Without a use statement
require 'path/to/lib/shoprunback-php/init.php';

$brand = new \Shoprunback\Elements\Brand();
```

The class Brand represents a brand.

All your **products are linked to a brand**. If you **forgot to link** a product to a brand, it is **then automatically linked to the default** brand.

#### Parameters

> Create a Brand

```php
<?php
require 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$brand = new \Shoprunback\Elements\Brand();

// Mandatory to create a new brand
$brand->name = 'My super brand';
$brand->reference = 'my-super-brand';

// Now we can save the brand!
$brand->save();
```

Parameter | Required to create | Type | Description | Tips
-|-|-|-|-
**name** | Yes | **String** | Name of the brand, displayed to the customer on the return process
**reference** | Yes | **String** | Unique reference of the brand | Use only lowercase letters and replace spaces by -

#### API operations

> Get all Brands (paginated)

```php
<?php
require 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$brands = \Shoprunback\Elements\Brand::all();
```

> Get one Brand

```php
<?php
require 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$brand = \Shoprunback\Elements\Brand::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');
```

> Update a Brand

```php
<?php
require 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$brand = \Shoprunback\Elements\Brand::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');
$brand->name = 'New name';
$brand->save();
```

> Delete a Brand

```php
<?php
require 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

// Get the brand and remove it later in the code
$brand = \Shoprunback\Elements\Brand::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');

// Delete a brand by an instance
$brand->remove();

// --
// OR
// --

// Delete a brand directly
\Shoprunback\Elements\Brand::delete('1f27f9d9-3b5c-4152-98b7-760f56967dea');
```

Operation | Enabled
-|-
**Get all (paginated)** | Yes
**Get one** | Yes
**Create** | Yes
**Update** | Yes
**Delete** | Yes