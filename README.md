# Hashidable

> Hashidable uses [hashids](https://hashids.org/) to obfuscate Laravel route ids.

## Installation

*Note: This package is built to work with Laravel versions greater than 7. It may work in older version, but this has not been tested.*

```
composer require kayandra/hashidable
```

## Setup

Import the `Hashidable` trait and add it to your model.

```php
use Kayandra\Hashidable\Hashidable;

Class User extends Model
{
  use Hashidable;
}
```

## Usage

```php
$user = User::find(1);

$user->id; // 1
$user->hashid; // 3RwQaeoOR1E7qjYy

User::find(1);
User::findByHashId('3RwQaeoOR1E7qjYy');
User::findByHashidOrFail('3RwQaeoOR1E7qjYy');
```

### Route Model Binding

Assuming we have a route resource defined as follows:

```php
Route::apiResource('users', UserController::class);
```

This package does not affect route model bindings, the only difference is, instead of placing the id in the generated route, it uses the hashid instead.

So, `route('users.show', $user)` returns `/users/3RwQaeoOR1E7qjYy`;

When you define your controller that auto-resolves a model in the parameters, it will work as always.

```php
public function show(Request $request, User $user)
{
  return $user; // Works just fine
}
```

## FAQs

<details>
  <summary>Where are the generated hashes stored?</summary>

  Hashidable does not touch the database to store any sort of metadata. What it does instead is use an internal encoder/decoder to dynamically calculate the hashes.
</details>

<details>
  <summary>Will this improve security?</summary>

  No. hashids are meant to provide obscurity in the URL.

  Relying on _obscurity for security is not advisable_, this package is meant for aesthetics.

  If you like YouTube style URLs, then by all means use this package.
</details>

## License

[MIT](/LICENSE.md)
