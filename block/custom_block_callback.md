### Ease the use of block view callback.

Read this ticket for the full story and core patch attempt: https://www.drupal.org/node/2651996

The 2 variants are similar on how to use it. Each block can define a custom callback and file to store the callback.

## Variant 1
It requires each module to implement a custom hook_block_view(). Only the hook_block_view() from the module defining the block is called.

```php
/**
 * Implements hook_block_info().
 */
function MY_MODULE_block_info() {
  $blocks = array();

  $blocks['my_block'] = array(
    'info' => t('My block'),
    'callback' => 'MY_MODULE_my_block_view',
    'file' => 'my_module.block.inc',
  );

  return $blocks;
}

/**
 * Implements hook_block_view().
 */
function MY_MODULE_block_view($delta) {
  $block = array();

  $definitions = MY_MODULE_block_info();

  if (isset($definitions[$delta]['callback'])) {
    if (isset($definitions[$delta]['file'])) {
      require_once $definitions[$delta]['file'];
    }

    $block = call_user_func($definitions[$delta]['callback']);
  }

  return $block;
}
```

## Variant 2
A custom module implements a hook_block_view_alter() which does the trick for all the modules. It requires this hook_block_view_alter() to be the first called so other alterating can be done as usual.

Generic custom module
```php
/**
 * Implements hook_block_view_alter().
 */
function MY_SECOND_MODULE_block_view_alter(&$data, $block) {
  // If the block is empty, that means the module providing the module has not
  // implemented the hook_block_view().
  if (empty($data)) {
    $definitions = module_invoke($block->module, 'block_info');

    if (isset($definitions[$block->delta]['callback'])) {
      if (isset($definitions[$block->delta]['file'])) {
        $ext = pathinfo($definitions[$block->delta]['file'], PATHINFO_EXTENSION);
        module_load_include($ext, $block->module, drupal_substr($definitions[$block->delta]['file'], 0, - (drupal_strlen($ext) + 1)));
      }

      $data = call_user_func($definitions[$block->delta]['callback']);
    }
  }
}

// @TODO: Implements hook_module_implements_alter() so set this hook implementation the first in the list.
```

Custom module defining blocks
```php
/**
 * Implements hook_block_info().
 */
function MY_MODULE_block_info() {
  $blocks = array();

  $blocks['my_block'] = array(
    'info' => t('My block'),
    'callback' => 'my_module_my_block_view',
    'file' => 'my_module.block.inc',
  );

  return $blocks;
}
```
