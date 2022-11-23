<button
  on:click={() => {
    workarounds = !workarounds
  }}
>
  {workarounds ? 'Disable' : 'Enable'} workarounds
</button>

<ul class:workarounds>
  <li data-i="0">0. No problems when we use a div directly instead of Component</li>
  <Item data-i="1">1. Only a problem when using a Component instead of a div.</Item>
  <Item data-i="2">2. Workaround 1: don't use &amp;.</Item>
  <Item data-i="3">
    3. Workaround 2: Use :global {'{}'} from svelte-preprocess
  </Item>
</ul>

<script lang="ts">
  import Item from './lib/Item.svelte'

  let workarounds = false
</script>

<style lang="scss">
  ul {
    --green: #1bb133;
  }

  :global(li) {
    // Adding pseudo-classes to the outer selector works fine -- Svelte does not add a suffix to isolate it
    /*
    &:hover {
      background-color: yellow !important;
    }
    */

    &[data-i='0'] {
      background-color: var(--green);
    }
    // Without :global it warns "Unused CSS selector" and removes it.
    // With :global it outputs incorrect output (preserves "&" as-is):
    //   li&[data-i='1'] {
    // instead of this:
    //   li[data-i='1']
    :global(&[data-i='1']) {
      background-color: var(--green);
    }
    &[data-i='2'] {
      background-color: var(--green);
    }
    &[data-i='3'] {
      background-color: var(--green);
    }
  }

  // Workaround 1: Don't use &
  :global(.workarounds li[data-i='1']) {
    background-color: var(--green);
  }
  :global(.workarounds li[data-i='2']) {
    background-color: var(--green);
  }

  // Workaround 2: Use :global {} from svelte-preprocess
  :global {
    .workarounds li {
      &[data-i='3'] {
        background-color: var(--green);
      }
    }
  }
</style>
