---
title: Views and Routes
author: Leven Rochana
date: 2025-06-30 18:22:48 +05:30
layout: post
---

Routes are defined in the `routes/web.php` file.

## '`GET`' Routes

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function() {
    return view('welcome');
});
```

- Above code tells Laravel to run the code defined inside the `function` if the user visits the **'/' page** (listens for a `GET` request).
- The `function` here returns a *view* called 'welcome' defined in `resources/views` directory as a *blade component* (`welcome.blade.php`).


```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function() {
    return view('welcome');
});

Route::get('/about', function() {
    return view('about');
});

Route::get('/contact', function() {
    return view('contact');
});
```

## Passing Data

Data can be passed to a *view* by passing it as the second argument as an *array* to the 'view' *function*.

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/jobs', function() {
    return view('jobs', [
        'jobs' => [
            [
                'title' => 'Director',
                'salary' => '$50,000',
            ],
            [
                'title' => 'Programmer',
                'salary' => '$10,000',
            ],
            [
                'title' => 'Teacher',
                'salary' => '$40,000',
            ],
        ]
    ]);
});
...
```

 - Data that are passed can be accessed in a *blade* file as a varible with the same name as the 'key' name.

 ```php
...
    @foreach ($jobs as $job)
        <li>{{ $job['title'] }}: Pays {{ $job['salary'] }}</li>
    @endforeach
...
 ```

 ## Wildcard Routes

A Wildcard Route is a route that matches any URL pattern, often used to catch unknown pages or define dynamic routes that accept a variable.

```php
<?php

use Illuminate\Support\Arr;
use Illuminate\Support\Facades\Route;

Route::get('/jobs/{id}', function($id) {
    $jobs = [
            [
                'id' => 1,
                'title' => 'Director',
                'salary' => '$50,000',
            ],
            [
                'id' => 2,
                'title' => 'Programmer',
                'salary' => '$10,000',
            ],
            [
                'id' => 3,
                'title' => 'Teacher',
                'salary' => '$40,000',
            ],
    ];
    Arr::first($jobs, fn($job) => $job['id'] == $job);
    return view('job', ['job' => $job])
});
...
```

 - A Wildcard Route can be defined with the '{}' appended to the route name.
 - A Route Varible is passed inside '{}' that can be used to run conditional code (`{id}`).
    - This code passes the corresponding 'job' to the *view* by iteratoring over every element in the *array* and checking if the **route variable** matches the **id** of the 'job'

    ```php
    ...
        Arr::first($jobs, fn($job) => $job['id'] == $job);
        return view('job', ['job' => $job])
    ...
    ```
