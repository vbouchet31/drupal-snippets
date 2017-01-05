### Provide an ajax menu delivery callback which result is cached.

```php
/**
 * Custom delivery_callback to enable page cache.
 *
 * We use this as ajax_deliver() don't deal with caching.
 */
function MY_MODULE_ajax_deliver($page_callback_result) {
  // Use the default ajax_deliver to set appropriate headers.
  ajax_deliver($page_callback_result);

  // Ensure it is cached.
  drupal_page_footer();
}
```

### Example
```php
/**
 * Implements hook_menu().
 */
function MY_MODULE_menu() {
  $items = array();
  
  $items['my/path'] = array(
    'access arguments' => array('access content'),
    'page callback' => 'my_page_callback',
    'delivery callback' => 'MY_MODULE_ajax_deliver',
    'type' => MENU_CALLBACK,
  );
  
  return $items;
}
```

The function MY_MODULE_ajax_deliver() must be in a .module or it won't be available.