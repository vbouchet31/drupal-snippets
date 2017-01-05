### Provide a custom function which does the same thing as format_plural() WITHOUT translating the given string.

It may be useful when generating an URL for example.

```php
/**
 * Format plural without translating.
 *
 * @see format_string()
 */
function MY_MODULE_format_plural($count, $singular, $plural, array $args = array()) {
  $args['@count'] = $count;
  if ($count == 1) {
    return format_string($singular, $args);
  }

  // Get the plural index through the gettext formula.
  $index = (function_exists('locale_get_plural')) ? locale_get_plural($count, NULL) : -1;
  // If the index cannot be computed, use the plural as a fallback (which
  // allows for most flexiblity with the replaceable @count value).
  if ($index < 0) {
    return format_string($plural, $args);
  }
  else {
    switch ($index) {
      case '0':
        return format_string($singular, $args);

      case '1':
        return format_string($plural, $args);

      default:
        unset($args['@count']);
        $args['@count[' . $index . ']'] = $count;
        return format_string(strtr($plural, array('@count' => '@count[' . $index . ']')), $args);
    }
  }
}
```

### Example
```php
$str = MY_MODULE_format_plural(10, 'Only one value (@value)', 'Multiple values (@value)', array('@value' => 10); // Return "Multiple values (10)" even in a French context for example.
$str = format_plural(10, 'Only one value (@value)', 'Multiple values (@value)', array('@value' => 10); // Return "Plusieurs valeurs (10)" in a French context for example.
```
