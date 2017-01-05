### Check if the first given tid has the second given tid as parent.

```php
/**
 * Utility function to check if a given taxonomy as the given tid as parent.
 */
function MY_MODULE_taxonomy_term_has_parent($children_tid, $parent_tid) {
  if (is_object($children) {
    $children = $children->tid;
  }
  if (is_object($parent) {
    $parent = $parent->tid;
  }

  $parents = taxonomy_get_parents_all($children);
  $parents = array_reverse($parents);

  foreach ($parents as $tmp) {
    if ($tmp->tid == $parent) {
      return TRUE;
    }
  }

  return FALSE;
}
```
