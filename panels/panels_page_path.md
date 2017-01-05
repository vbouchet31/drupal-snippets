### Get the path of a page from its page name. It may be useful when you let contributors determine the path of a panel page but you need the value within the code for links.

```php
/**
 * The function returns the path of the panel page.
 *
 * @param string $page_id
 *   The panel page id.
 */
function MY_MODULE_get_panels_page_path($page_id) {
  $path = '';

  ctools_include('export');
  $result = ctools_export_load_object('page_manager_pages', 'names', array($page_id));

  if (!empty($result) && !empty($result[$page_id])) {
    $path = $result[$page_id]->path;
  }

  return $path;
}
```

/!\ It may not always be applicable and has only been tested for now on pages with one variant only.