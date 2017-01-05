### Set the page status to enabled or disabled.

```php
/**
 * Helper function to enable or disable a page.
 *
 * See page_manager_enable_page() in page_manager.admin.inc.
 */
function MY_MODULE_panel_page_status($page_id, $status = TRUE) {
  $page = page_manager_cache_load('page-' . $page_id);
  if (!empty($page) && $function = ctools_plugin_get_function($page->subtask, 'enable callback')) {
    $function($page, !$status);
    menu_rebuild();
  }
}
```

### Example
```php
MY_MODULE_panel_page_status('my_page', TRUE); // Enable the panel page.
```