### Programmaticaly set the metatags on a Panels page.`

It may be useful in multisite context where the metatags are different per website and can't be part of the exported code (features or other).

```php
/**
 * Helper function to set the metatags on a page.
 *
 * To easily get the $task_id and $subtask_id, you can check in the exported
 * code of the page you want to edit.
 *
 * @param string $task_id
 *   The name of task to load (page, node_view).
 *   See ctools/page_manager/plugins/tasks/.
 * @param string $subtask_id
 *   The name of the subtask to load.
 * @param array $metatags
 *   An array of metatags to apply.
 */
function MY_MODULE_set_panels_metatags($task_id, $subtask_id, $metatags = array()) {
  $task = page_manager_get_task($task_id);
  if (empty($task)) {
    return;
  }

  $handlers = page_manager_load_sorted_handlers($task, $subtask_id, TRUE);
  if (empty($handlers)) {
    return;
  }

  // For now we only deal with the first handler (variant).
  $handler = reset($handlers);

  // Apply the given metatags.
  foreach ($metatags as $key => $value) {
    $handler->conf['metatag_panels']['metatags'][LANGUAGE_NONE][$key]['value'] = $value;
  }

  // Be sure the metatags are translatable.
  metatag_translations_update($handler->conf['metatag_panels']['metatags'], 'metatag_panels:' . $handler->name);

  // Save the Panels with the new metatags.
  page_manager_save_task_handler($handler);
}
```

### Example
```php
$metatags = array(
  'title' => 'The page title',
  'description' => 'The page description',
);
MY_MODULE_set_panels_metatags('page', 'my_page_name', $metatags);
``