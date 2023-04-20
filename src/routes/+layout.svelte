<script>
  import { onMount } from "svelte";
  import "../app.scss";
</script>

<div class="container">
  <slot />
</div>

<style lang="scss">
  .container {
    position: relative;
    min-height: calc(100vh + 1px);
    z-index: 1;
    margin: 0 6vw;
    border-left: $line;
    border-right: $line;
    @media screen and (max-width: $breakpoint-4-1) {
      margin: 0 2rem;
    }
    @media screen and (max-width: $breakpoint-2-1) {
      margin: 0 0.8rem;
    }
  }
</style>
