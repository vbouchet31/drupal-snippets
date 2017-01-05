### Change the keys of an array to get items indexed by a value from the item object.

```php
/**
 * Utility function to index array based on specified key.
 *
 * @param array $array
 *   Array to be indexed.
 * @param string $key
 *   Key to index the array on.
 *
 * @return array
 *   Indexed array.
 */
function MY_MODULE_get_indexed_array(array $array, $key) {
  $items = array();

  foreach ($array as $row) {
    if (is_object($row)) {
      $items[$row->{$key}] = $row;
    }
    else {
      $items[$row[$key]] = $row;
    }
  }

  return $items;
}
```

### Example
```php
$array = array(
  'val1' => array(
    'attribute1' => 1,
    'attribute2' => 'c',
  ),
  'val2' => array(
    'attribute1' => 10,
    'attribute2' => 'b',
  ),
  'val3' => array(
    'attribute1' => 5,
    'attribute2' => 'a',
  ),
);

$new_array = MY_MODULE_get_indexed_array($array, 'attribute1'); // 1, 10, 5.
$new_array = MY_MODULE_get_indexed_array($array, 'attribute2'); // 'c', 'b', 'a'.
```

It works both for array of array and array of objects.