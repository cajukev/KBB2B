<script>
  import {setContext} from 'svelte';
  setContext('lang', 'en');
  import Header from '$lib/components/header.svelte';
</script>

<Header lang='en'></Header>
<slot/>
