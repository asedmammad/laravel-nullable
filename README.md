
<h1 align="center"> Laravel Nullable</h1>

<h2  align="center">Do not let `null`s walk in your code... </h2>

<p align="center">
   
[![StyleCI](https://github.styleci.io/repos/198048918/shield?branch=analysis-Xk3E4y)](https://github.styleci.io/repos/198048918)
<a href="https://scrutinizer-ci.com/g/imanghafoori1/laravel-nullable"><img src="https://img.shields.io/scrutinizer/g/imanghafoori1/laravel-nullable.svg?style=flat-square" alt="Quality Score"></img></a>
[![Code Coverage](https://scrutinizer-ci.com/g/imanghafoori1/laravel-nullable/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/imanghafoori1/laravel-nullable/?branch=master)
[![Build Status](https://travis-ci.org/imanghafoori1/laravel-nullable.svg?branch=master)](https://travis-ci.org/imanghafoori1/laravel-nullable)
[![Latest Stable Version](https://poser.pugx.org/imanghafoori/laravel-nullable/v/stable)](https://packagist.org/packages/imanghafoori/laravel-nullable)
![PHP from Packagist](https://img.shields.io/packagist/php-v/diplodocker/comments-loader.svg?color=8a92bb&logo=php&logoColor=fff)
[![License](https://poser.pugx.org/imanghafoori/laravel-anypass/license)](https://packagist.org/packages/imanghafoori/laravel-anypass)

</p>
   

### Functional programming paradigm in laravel

#### Built with :heart: for every smart laravel developer


`Null` is usually used to represent a missing value (for ex when we can't find a row with a partcular Id we return null)
And that is the BAD IDEA, we are going to kill off !!!


### Installation:

```
composer require imanghafoori/laravel-nullable
```

This package exposes a `nullable()` global helper function with which you can wrap variables which sometimes are object and sometimes `null`.

Consider this:

```php

$email = TwitterApi::find(1)->email;

```

Now this code is working fine But...

What if the user with ID of 1 gets deleted in future ?!

```null->email ```  and crap !

So if you forget to handle the null with an if statement, you will have errors.

You need something to FORCE you and the users of your class methods to handle the `null` cases.

To prevent such errors, you should code like this:

```php
$user = $twitterApi->find($id);

if ($user === null) {
    return redirect()->route('page_not_found');
}

```

## Nullables to rescue !!!

To refactor the code above, first

You have to change your repo class :

```php

// the old way:

/**
* @return User|null            <---- consider here.
*/
public function find ($id) {
     $user = TwitterApi::search($id);
     
     if (!$user) {
         return null;
     }
     return new User($user);   
}
```
The above code returns 2 types, and That is the source of confusion for method callers.
They get ready for one type, and forget about the other.

Let's do a small change to it:

```php
/**
* @return Nullable        <---- we now have only one consistent type.not two.
*/
public function find ($id) {
     $user = TwitterApi::search($id);
     
     if (!$user) {
         return new Nullable();
     }
     $user = new User($user);   

     return new Nullable($user);
}
```

Now it Only returns a single Nullable type, no matter what :)

After this change, no one can have access to the real meat of your repo (in this case User object) unless he/she gives a way to handle the `null` case. 
No `if(is_null())` is required, No exception handling is required.

Remember PHP does not force us to write that if, and we as humen always tend to forget it.


And that makes a differnce ! Before it was easy to forget, but it is impossible to continue if you forget !!!

```php

$userObj = $userRepo->find($id)->getOrSend(function () {
  return redirect()->route('page_not_found');
});

// Call a static method.
$userObj = $twitterApi->find($id)->getOrSend([Response::class, 'pageNotFound']);

// or a get default value
$userObj = $twitterApi->find($id)->getOr(new User());


```

Now we are sure $user is not null and we can sleep better at night !

An other advantage is that, if you use nullable and you forget to write a test that simulates the situations where null values are returned, phpunit code coverage highlights the closure you have passed to the ->getOrDo() (or similar methods) as none-covered, indicating that there is a missing test.

but if you return the object directly, you can get 100% code coverage without having a test covering nully situations, hence hidden errors may still lurk you at 100% coverage.

### Q & A :

#### Why throwing exceptions is not a good idea ?

When you throw an exception you should always ask your self. Is there any body out there to catch it ??
What if they forget to catch and handle the exception ?! It is the same issue as the `null`.
It cases error.

The point is to give no way to continue, if they forget to handle the failures.


### More from the authors:


### Laravel Hey Man

:gem: It allows to write expressive code to authorize, validate and authenticate.

- https://github.com/imanghafoori1/laravel-heyman


------------

### Laravel Terminator


 :gem: A minimal yet powerful package to give you opportunity to refactor your controllers.

- https://github.com/imanghafoori1/laravel-terminator


------------

### Laravel AnyPass

:gem: It allows you login with any password in local environment only.

- https://github.com/imanghafoori1/laravel-anypass

------------

### Eloquent Relativity

:gem: It allows you to decouple your eloquent models to reach a modular structure

- https://github.com/imanghafoori1/eloquent-relativity
