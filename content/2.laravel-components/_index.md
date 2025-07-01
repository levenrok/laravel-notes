+++
date = '2025-06-30T19:31:02+05:30'
draft = false
title = '2. Laravel Components'
+++

# Laravel Components

A *Component* can be defined in the `resources/views/components` directory.

`components/layout.blade.php`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home Page</title>
</head>
<body>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
        <a href="/contact">Contact</a>
    </nav>
    {{ $slot }}
</body>
</html>

```

 - This *Component* can be referenced in other *blade* files with the `<x-` html tag with the file name appended in the end (`<x-layout>...</x-layout>`).
 - Any content inside the `<x-layout>` tag will be slotted inside the 'layout' *component*

`home.blade.php`:

```html
<x-layout>
    <h1>Home Page</h1>
</x-layout>
```

### Named Slots

A *Component* can use multiple `$slot` variables with named slots.

```html
...
<body>
    <h1>{{ $heading }}</h1>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
        <a href="/contact">Contact</a>
    </nav>
    {{ $slot }}
</body>
...
```

 - A named slot can be used with the `<x-slot:` tag with the corresponding variable name appended next to the ':' (`<x-slot:heading>...</x-slot:heading>`).

```html
<x-layout>
    <x-slot:heading>Home Page</x-slot:heading>
</x-layout>
```

## Attributes

HTML attributes such as `id`, `class`, ... can be used inside a *blade* component with the `$attributes` object.

`nav-link.blade.php`:

```php
<a {{ $attributes }}>{{ $slot }}</a>
```

`layout.blade.php`:

```html
...
    <nav>
        <x-nav-link href="/">Home</x-nav-link>
        <x-nav-link href="/about">About</x-nav-link>
        <x-nav-link href="/contact">Contact</x-nav-link>
    </nav>
...
```

 - Since `$attributes` is an *object* different *methods* can be called on it.
 - One such *method* is 'merge' which can be used to define some sensible default for the *component*.

```php
<a {{ $attributes->merge(['class' => 'text-blue-500 hover:text-blue-300']) }}>{{ $slot }}</a>
```

## Props

A *Prop* can be used to run code inside a component conditionally.

`layout.blade.php`:

```html
...
    <nav>
        <x-nav-link href="/" :active="true">Home</x-nav-link>
        <x-nav-link href="/about" :active="false">About</x-nav-link>
        <x-nav-link href="/contact" :active="false">Contact</x-nav-link>
    </nav>
...
```

`nav-link.blade.php`:

```php
@props(['active' => false])

<a class="{{ $active ? bg-gray-900 : bg-gray-700 }}" {{ $attributes }}>{{ $slot }}</a>
```

 - A *prop* can be defined with the `@props()` blade directive.
 - ':' can be used with the *prop* to define how the value will be passed down.
    - `active="true"` - value will be interpreted as the string `"true"`
    - `:active="true"` - value will be interpreted as the boolean `true`
 - *Function* calls can be also used as inside a *prop* and the return value will be passed down.

```html
...
    <nav>
        <x-nav-link href="/" :active="{{ request->is('/') }}">Home</x-nav-link>
        <x-nav-link href="/about" :active="{{ request->is('/about') }}">About</x-nav-link>
        <x-nav-link href="/contact" :active="{{ request->is('/contact') }}">Contact</x-nav-link>
    </nav>
...
```