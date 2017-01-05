### Access callback for menu items. Receive an array of permissions as argument and return TRUE if the current user has at least one of the given permissions.

```php
/**
 * Determine whether the user has at least one privilege in the given list.
 */
function user_access_or($permissions) {
  foreach ($permissions as $permission) {
    if (user_access($permission)) {
      return TRUE;
    }
  }

  return FALSE;
}
```

####Example
````php
/**
 * Implements hook_menu().
 */
function MY_MODULE_menu() {
  $items = array();

  $items['my/path'] = array(
    'title' => 'The page title',
    'access callback' => 'user_access_or',
    'access arguments' => array(array('administer users', 'custom permission')),
  );

  return $items;
}
```

The user will granted access to 'my/path' if he has 'administer users' permission or 'custom permission' assigned to his role.
