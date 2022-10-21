<script>
  import {setContext} from 'svelte';
  setContext('lang', 'fr');
  import Header from '$lib/components/header.svelte';
</script>

<Header lang='fr'></Header>
<slot/>
