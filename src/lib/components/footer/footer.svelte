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

    @media screen and (max-width: $breakpoint-2-1) {
      flex-direction: column;
      align-items: center;
    }
    & p {
      margin: 0;
      margin-left: 2rem;
      @media screen and (max-width: $breakpoint-3-1) {
        margin-left: 1rem;
      }
      @media screen and (max-width: $breakpoint-2-1) {
        margin-left: 0;
        margin-top: 0.25rem;
      }
    }
  }
</style>
