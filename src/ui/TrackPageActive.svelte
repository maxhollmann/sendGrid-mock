<script>
  import { pageActive } from './stores.js';

  function onPageFocus() {
    pageActive.set(true)
  }

  function onPageBlur() {
    pageActive.set(false)
  }
</script>

<svelte:window on:focus={onPageFocus}
               on:blur={onPageBlur}
               />
