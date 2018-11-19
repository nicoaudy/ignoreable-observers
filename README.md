# Ignorable Observers

Dynamically disable/enable Laravel's Eloquent model observers. This library provides the ability to temporarily disable observable events for Eloquent models. For example, temporarily disable observers that kick off emails, push notifications, or queued calculations when performing a large number of database inserts or updates.

## Installation

Install using composer:

```
composer require nicoaudy/ignoreable-observers
```

## Usage

To give an Eloquent model the ability to temporarily ignore observers, simply add the `IgnorableObservers` trait:

```php
<?php namespace App;

use Illuminate\Database\Eloquent\Model;
use IgnorableObservers\IgnorableObservers;

class ExampleModel extends Model {
  use IgnorableObservers;
}
```

Then, call the `ignoreObservableEvents()` static method to ignore all observers for that model:

```php
ExampleModel::ignoreObservableEvents();
```

The `ignoreObservableEvents()` method also accepts an array of observers to be ignored. For example, the following line would ignore only the `saved` and `created` observers:

```php
ExampleModel::ignoreObservableEvents(['saved', 'created']);
```

To stop ignoring a model's observers, call the `unignoreObservableEvents()` static method:

```php
ExampleModel::unignoreObservableEvents();
```

The `unignoreObservableEvents()` method _also_ accepts an array of observers to unignore, giving you total control over which observers to enable and disable:

```php
ExampleModel::unignoreObservableEvents(['saved']);
```

### Example

The following example ignores any `saved` and `created` observers for the `ExampleModel`, inserts 100 rows into the database, and then "unignores" those observers when the operation is completed:

```php
ExampleModel::ignoreObservableEvents('saved', 'created');

for ( $i = 0; $i <= 100; $i++ ) {
  ExampleModel::create([
    'data' => $i
  ]);
}

ExampleModel::unignoreObservableEvents();
```

## License

Ignorable Observers is an open-sourced library licensed under the MIT license.
