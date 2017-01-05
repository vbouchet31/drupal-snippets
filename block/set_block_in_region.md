### Programmaticaly set a block in a region of a theme.

```php
/**
 * Utility function to configure blocks in required regions.
 */
function MY_MODULE_configure_block_region($module, $block, $region, $theme = '', $pages = '', $visibility = 0, $weight = 0, $title = '') {
  if (empty($theme)) {
    $theme = variable_get('theme_default');
  }

  $fields = array(
    'region' => ($region == BLOCK_REGION_NONE ? '' : $region),
    'pages' => $pages,
    'status' => (int) ($region != BLOCK_REGION_NONE),
    'visibility' => (int) $visibility,
    'weight' => $weight,
  );

  if (!empty($title)) {
    $fields['title'] = $title;
  }

  db_merge('block')
    ->key(array('theme' => $theme, 'delta' => $block, 'module' => $module))
    ->fields($fields)
    ->execute();
}
```

### Example
```php
MY_MODULE_configure_block_region('MY_MODULE', 'BLOCK_DELTA', 'MY_REGION', 'MY_THEME');
MY_MODULE_configure_block_region('MY_MODULE', 'BLOCK_DELTA', 'MY_REGION', 'MY_THEME', '', 0, 0, '<none>'); // Hide the default block title.
```
