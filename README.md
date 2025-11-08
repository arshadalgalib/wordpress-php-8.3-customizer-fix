# WordPress Customizer Fix for PHP 8.3

Fixes the **“There has been a critical error on this website”** issue when opening **Appearance → Customize** on **WordPress 6.8+** with **PHP 8.3**.

---

## Problem

After upgrading to PHP 8.3, many WordPress users encounter this error:


### Cause

When there are no widgets active, WordPress sometimes returns `false` for `$sidebars_widgets`.  
However, PHP 8.3 no longer allows `array_merge()` to be called on a boolean value —  
so the Customizer immediately crashes.

---

## The Fix (Add to `functions.php`)

Simply add this snippet to your active theme’s `functions.php` file:

```php
// 🛠️ Fix Customizer crash on PHP 8.3 with empty widgets
add_filter( 'sidebars_widgets', function( $sidebars_widgets ) {
    if ( $sidebars_widgets === false ) {
        $sidebars_widgets = [];
    }
    return $sidebars_widgets;
});


