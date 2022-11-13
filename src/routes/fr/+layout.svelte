<script>
  import {setContext} from 'svelte';
  setContext('lang', 'fr');
  import Header from '$lib/components/header/header.svelte';
  import Footer from '$lib/components/footer/footer.svelte';
</script>

<Header lang='fr'></Header>
<slot/>
<Footer lang='fr'></Footer>