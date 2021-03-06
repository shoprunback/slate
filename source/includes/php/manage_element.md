# Manage a single Element

## Use the library's classes

> Calling the class with a use statement

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

use \Shoprunback\Elements\Product as LibProduct;

$product = new LibProduct();
```

> Calling the class without a use statement

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = new \Shoprunback\Elements\Product();
```

You can either **call the class directly with its namespace** or write a **use statement** so you can **access it with the class name** only.

We recommend you to **use the use statement for your classes** needing one of the library's classes, since you will often call them.

For **files calling the library's classes rarely**, you can use the **direct call with the namespace**.

## Original values

> Get original values

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = \Shoprunback\Elements\Product::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');

echo $product->label; // Prints: 'My label'

$product->label = 'New label';

echo $product->label; // Prints: 'New label'

echo $product->_origValues->label; // Prints 'My label'

$product->refresh();

echo $product->label; // Prints 'My label'

$product->label = 'New label';

$product->save();

echo $product->_origValues->label; // Prints 'New label'
```

You can get the original values of an Element whenever you want by getting the `_origValues` attribute.

<aside class="warning">
<b>Never edit the</b> `_origValues`, they are the current value on the server.
</aside>

If you want to reset an element with its original values, use `refresh` (makes an API call)

<aside class="warning">
When the <b>element is saved</b>, the <b>origValues</b> will be <b>updated with the new values!</b>
</aside>

## How to delete an Element

> Remove an Element by an instance

```php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$brand = \Shoprunback\Elements\Brand::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');
$brand->remove();
```

> Remove an Element directly with its ID

```php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

\Shoprunback\Elements\Brand::delete('1f27f9d9-3b5c-4152-98b7-760f56967dea');
```

You **can** either **delete an Element by an instance with** `remove` **or directly with its ID with** `delete`.

## Get different attributes

> Different ways to get important attributes

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = new \Shoprunback\Elements\Product();

$product->label = 'My label';
$product->custom = 'My custom param';
$product->brand = new \Shoprunback\Elements\Brand();

$product->getAllAttributes(); // Returns 'label', 'custom' and 'brand' with their values
$product->getApiAttributes(); // Returns 'label' and 'brand' with their values
```

To get **all the attributes of an Element including the nested Elements**, you can use `getAllAttributes`.

To get **only the attributes an Element will use for an API call**, you can use `getApiAttributes`.

> Get all keys an Element accepts

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = new \Shoprunback\Elements\Product();

$product->getApiAttributesKeys(); // Returns all attributes a Product will accept before doing an API call
```

You can **list all the attributes an Element accepts when making an API call** with `getApiAttributesKeys`.

## Use the elements with my objects

> Separate my own classes and the library's classes

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

use \Shoprunback\Elements\Product as libProduct;

class myProduct
{
  public function __construct()
  {
    $this->label = 'My label';
    $this->reference = 'my-label';
    $this->weight_in_grams = 1000;
  }

  public function generateLibProduct()
  {
    $product = new libProduct();

    $product->label = $this->label;
    $product->reference = $this->reference;
    $product->weight_in_grams = $this->weight_in_grams;

    //[...]
    // Add all params to $product
    //[...]

    return $product;
  }

  public function save()
  {
    // [...]

    $libProduct = $this->generateLibProduct();
    $libProduct->save();

    // [...]
  }
}

$myProduct = new myProduct();
$myProduct->save();
```

> Extend the library's classes

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

use \Shoprunback\Elements\Product as libProduct;

class myProduct extends libProduct
{
  public function __construct($params)
  {
    $this->label = $params['label'];
    $this->reference = $params['reference'];
    $this->weight_in_grams = $params['weight_in_grams'];

    //[...]
    // Add all params to $product
    //[...]

    if (isset($params['id']) && !is_null($params['id'])) {
      parent::__construct($params['id']);
    } else {
      parent::__construct();
    }
  }

  // Be careful not to overwrite the functions of the parent class!
  public function saveProduct()
  {
    // Code...

    $this->save(); // save() is a function of the parent class, don't overwrite it!

    // Code...
  }
}

$params = [
  'label' => 'My label',
  'reference' => 'my-label',
  'weight_in_grams' => 1000,
];

$myProduct = new myProduct($params);
$myProduct->saveProduct();
```

You can **either use the library independantly of your classes or extend the library's classes**

<aside class="warning">
<b>If you use inheritance</b>, be <b>careful not to overwrite the parent functions</b>. Instead, <b>use functions including the parent function</b>.
</aside>

## Check changed fields

> Check if an Element is a new one or not

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = new \Shoprunback\Elements\Product();
$product->isPersisted(); // false

$product->label = 'New label';
$product->reference = 'new-label';
$product->weight_in_grams = 1000;
$product->isPersisted(); // false

$product->save();
$product->isPersisted(); // true

$retrievedProduct = \Shoprunback\Elements\Product::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');
$retrievedProduct->isPersisted(); // true
```

> Check changed fields in a existing Product

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = \Shoprunback\Elements\Product::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');

$product->isDirty(); // false
$product->getDirtyKeys(); // Returns []
$product->isKeyDirty('label'); // false

$product->label = 'New label';

$product->isDirty(); // true
$product->getDirtyKeys(); // Returns ['label']
$product->isKeyDirty('label'); // true
```

> Check changed fields in a new Product

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\RestClient::getClient()->setToken('yourApiToken');

$product = new \Shoprunback\Elements\Product();
$product->isDirty(); // true

$product->label = 'My label';
$product->isDirtyKey('label'); // true
$product->getDirtyKeys(); // Returns ['label']
```

To know **if an Element is new**, you can use `isPersisted`. If it **returns true**, then the Element is **not new**.

You can **check if an Element is new or has at least one parameter changed** with `isDirty`.

You can know **which keys have been changed** with `getDirtyKeys`.

You can also check **if a precise key has changed** with `isDirtyKey`.

When you try to **update an existing Element**, it will **only send the attributes that has been changed**.

A **key present in the** `getApiAttributesKeys` that **hasn't a** `_origValues` is **considered changed** (ex: when you create a new Product and add a label).

## Which API calls can it do ?

> Check which API calls an Element can or cannot do

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

// All those methods return true or false
\Shoprunback\Elements\Product::canRetrieve(); // Check if you can get one element
\Shoprunback\Elements\Product::canCreate(); // Check if you can save a new element
\Shoprunback\Elements\Product::canUpdate(); // Check if you can update an existing element
\Shoprunback\Elements\Product::canDelete(); // Check if you can delete an element
\Shoprunback\Elements\Product::canGetAll(); // Check if you can get many elements at once
```

You can **check if an element can do an API call** with `canRetrieve()`,`canCreate()`, `canUpdate()`, `canDelete()`, `canGetAll()`

## Get the element name

> Get the element name

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

\Shoprunback\Elements\Address::getElementName(); // Returns 'address'
\Shoprunback\Elements\Brand::getElementName(); // Returns 'brand'
\Shoprunback\Elements\Item::getElementName(); // Returns 'item'
\Shoprunback\Elements\Order::getElementName(); // Returns 'order'
\Shoprunback\Elements\Product::getElementName(); // Returns 'product'
\Shoprunback\Elements\Shipback::getElementName(); // Returns 'shipback'
\Shoprunback\Elements\Warehouse::getElementName(); // Returns 'warehouse'
```

To easily **get** the **lowercase name of an element**, you can use those methods.

> Get a nested element with its name

```php
<?php
require_once 'path/to/lib/shoprunback-php/init.php';

$product = \Shoprunback\Elements\Product::retrieve('1f27f9d9-3b5c-4152-98b7-760f56967dea');

$brandName = \Shoprunback\Elements\Brand::getElementName(); // $brandName = 'brand'

$product->$brandName; // $product->brand
```

This way, you can easily **get the attribute name for a nested Element**. It can be **useful in recursive functions or logs**.