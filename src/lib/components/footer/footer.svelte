<script>
  import translation from "./footer.json";
  export let lang;
  let t = translation[lang];
</script>

<div class="container">
  <p>{t["Title"]}</p>
  <p>{t["Location"]}</p>
  <p>{t["Phone"]}</p>
  <p>{t["Email"]}</p>
</div>

<style lang="scss">
  .container {
    display: flex;
    justify-content: end;
    padding-bottom: 1rem;
    padding-top: 0.25rem;
    font-size: 0.8rem;
    & p{
      margin: 0;
      margin-left: 2rem;
    }
  }
</style>