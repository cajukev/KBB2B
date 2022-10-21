<script>
  import { getContext, onMount } from "svelte";
  import translation from "./index.json";
  const lang = getContext("lang");
  let t = translation[lang];
  let firstScroll = false;

  let section2
  let scrollArrow

  onMount(() => {
    // Section 1 Stagger
    let titleWords = document.getElementById("section1").getElementsByClassName("stagger");
    console.log(titleWords);
    for (let i = 0; i < titleWords.length; i++) {
      titleWords[i].style.animationDelay = `${i * 0.25}s`;
    }
    // Extra delay for the last word
    titleWords[titleWords.length - 1].style.animationDelay = `${(titleWords.length - 1) * 0.25 + 0.25}s`;
    // First scroll event listener
    window.addEventListener('scroll', function() {
      if (firstScroll) return;
      firstScroll = true;
      section2.classList.remove("hiddenStart");
      scrollArrow.classList.add("animOut");
    });
  });
</script>

<div class="section" id="section1">
  <div class="squares">
    <div class="sq1 bp2" />
    <div class="sq2" />
    <div class="sq3" />
    <div class="sq4" />
    <div class="sq5 bp2" />
  </div>
  <div class="content">
    <h1>{@html t.title}</h1>
    <p class="fly-up">{t.description}</p>
  </div>
  <svg bind:this={scrollArrow} class="scrollArrow" viewBox="0 0 10 12" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M9.84308 6.21093C9.95108 6.06993 10.0001 5.91093 10.0001 5.75493C10.0001 5.36593 9.69408 4.99993 9.25108 4.99993H7.30482L9.843 1.711C9.951 1.57 10 1.411 10 1.255C10 0.866 9.694 0.5 9.251 0.5H0.75C0.305 0.5 0 0.867 0 1.255C0 1.412 0.0499999 1.571 0.159 1.712C0.848412 2.60256 1.81566 3.85143 2.70523 4.99993H0.750077C0.305077 4.99993 7.73668e-05 5.36693 7.73668e-05 5.75493C7.73668e-05 5.91193 0.0500773 6.07093 0.159077 6.21193C1.36208 7.76593 3.41108 10.4109 4.41708 11.7099C4.55908 11.8939 4.77708 11.9999 5.00908 11.9999C5.23908 11.9999 5.45808 11.8929 5.60008 11.7089L9.84308 6.21093ZM7.72508 6.49993H2.27908L5.00708 10.0219L7.72508 6.49993ZM7.725 2H2.279L4.60263 4.99993H5.40989L7.725 2Z" fill="#A6AEF2"/>
  </svg>  
</div>

<div class="section hiddenStart" id="section2" bind:this={section2}>
  <div class="frame">
    <h3>{ t['section2.title'] }</h3>
    <i>{ t['hook'] }</i>
    <p>{@html t['section2.list'] }</p>
  </div>
  <div class="images">
    <div>
      <div class="img1" />
      <div class="img2" />
      <div class="img3" />
    </div>
    <i>{ t['section2.quote'] }</i>
  </div>
</div>

<style lang="scss">
  .section {
    position: relative;
    padding-bottom: 2rem;

    &::after {
      content: "";
      display: block;
      position: absolute;
      bottom: 0;
      width: 100%;
      transform: scale(2, 1);
      transform-origin: center;
      height: 2px;
      background-color: white;
      z-index: -1;
      opacity: 0;
      animation: fly-up-separator 1.75s 2s cubic-bezier(0,.68,.59,1.01) forwards;
    }
  }
  #section1 {
    .squares {
      margin-top: 2rem;
      display: flex;
      align-items: center;
      justify-content: space-around;
      & div {
        &.sq3 {
          background: linear-gradient(135deg, #ff9c41 0%, #f27c0f 100%);
          box-shadow: 0px 0px 3rem rgba(233, 111, 0, 0.6);
          height: clamp(7rem, 20vw, 18rem);
          width: clamp(7rem, 20vw, 18rem);
          animation: spin 12s 2s ease-in-out infinite;
        }
      }
    }
    .content {
      margin-top: 2rem;
      width: 100%;
      text-align: center;
      color: $main-color;
      background: linear-gradient(180deg, rgba($bg1, 0) 0%, $bg1 22.92%, $bg1 71.87%, rgba($bg1, 0) 100%);
      transform: translateZ(0px);
      & p {
        margin-top: 1rem;
        opacity: 0;
        animation-delay: 2s;
      }
    }
    & .scrollArrow{
      position: absolute;
      bottom: -6rem;
      left: 50%;
      transform: translateX(-50%);
      width: 2rem;
      height: 2rem;
      opacity: 0;
      animation: fly-up-arrow 1.5s 2.5s cubic-bezier(0.25, 1, 0.5, 1) forwards, arrow-bounce 2s 4s ease-in-out infinite;
    }
  }
  
  #section2 {
    &.hiddenStart {
      opacity: 0;
    }
    &:not(.hiddenStart) {
      opacity: 1;
      animation: fade-up 1s 0s cubic-bezier(0,.68,.59,1.01) forwards;
    }
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 2rem;
    @media screen and (max-width: $breakpoint-2-1) {
      grid-template-columns: 1fr;
      padding: 0rem;
    }
    & .frame{
      padding: 1.5rem;
      width: fit-content;
      justify-self: center;
      & h3, p{
        max-width: 30rem;
      }
      & h3{
        margin-bottom: 1.25rem;
      }
      & p{
        margin-top: 0.75rem;
      }
      & li{
        margin-top: 0.25rem;
      }
    }
    & .images{
      
    }
  }
</style>
