<script>
  import {setContext} from 'svelte';
  setContext('lang', 'en');
  import Header from '$lib/components/header/header.svelte';
  import Footer from '$lib/components/footer/footer.svelte';
</script>

<Header lang='en'></Header>
<slot/>
<Footer lang='en'></Footer>
