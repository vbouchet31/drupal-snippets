### Sort an array of array or object based on a given key.

```php
/**
 * Function to sort an array based on a specific key.
 *
 * @param array $array
 *   Array to sort.
 * @param string $key
 *   Key of the array or object to sort with.
 * @param string $order
 *   Sorting order asc/desc.
 */
function MY_MODULE_usort(array &$array, $key, $order = 'desc') {
  usort($array, function($item_1, $item_2) use ($key, $order, $key2) {
    $val1 = is_array($item_1) ? $item_1[$key] : $item_1->$key;
    $val2 = is_array($item_2) ? $item_2[$key] : $item_2->$key;
    
    if ($val1 == $val2) {
        return 0;
      }
      if ($order == 'asc') {
        return ($val1 < $val2) ? -1 : 1;
      }
      elseif ($order == 'desc') {
        return ($val1 > $val2) ? -1 : 1;
      }
    }
  });
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

MY_MODULE_usort($array, 'attribute1'); // 'val2', 'val3', 'val1'.
MY_MODULE_usort($array, 'attribute1', 'asc'); // 'val1', 'val3', 'val2'.
MY_MODULE_usort($array, 'attribute2', 'asc'); // 'val3', 'val2', 'val1'.
```

It works both for array of array and array of objects.

