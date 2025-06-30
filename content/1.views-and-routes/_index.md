+++
date = '2025-06-30T18:22:48+05:30'
draft = false
title = '1. Views and Routes'
+++

# Views and Routes

Routes are defined in the `routes/web.php` file.

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

{{<mermaid>}}
stateDiagram-v2
    /: get('/', function() {...})
        note right of /
            return 'welcome.blade.php' view on GET request to '/'
        end note
    /about: get('/about', function() {...})
        note right of /about
            return 'about.blade.php' view on GET request to '/about'
        end note
    /contact: get('/contact', function() {...})
        note right of /contact
            return 'contact.blade.php' view on GET request to '/contact'
        end note
{{</mermaid>}}