<script>
  import { onMount } from "svelte";
  import "../app.scss";
  //PARALLAX by Max Paga / 2022

  // onMount(() => {
  //   let parallax = document.querySelectorAll(".parallax");
  //   let VH = window.innerHeight;
  //   let parallaxSpeed = 0.25;
  //   let posY = 0;

  //   parallax.forEach(function (p) {
  //     //variables
  //     if (!p.getAttribute("speed")) p.setAttribute("speed", parallaxSpeed);
  //     let speed = p.getAttribute("speed");
  //     let height = p.clientHeight;
  //     let scale = (VH - height) * speed;

  //     //attributes needed inside the scroll event
  //     p.setAttribute("calc", (VH - height) * 0.5);
  //     p.setAttribute("offset", p.getBoundingClientRect().top + window.pageYOffset);

  //     if (p.getAttribute("noscale") == null) {
  //       //element is scaled up to *exactly* fill the wrapper
  //       p.style.height = height + scale + "px";
  //       p.style.top = scale * -0.5 + "px";
  //     }

  //     //initialise position before scroll listener is called
  //     if (p.getAttribute("x") !== null) {
  //       //among X axis
  //       p.style.transform =
  //         "translate3d(" +
  //         (0 - p.getAttribute("offset") + parseInt(p.getAttribute("calc"))) * p.getAttribute("speed") +
  //         "px, 0, 0)";
  //     } else {
  //       //among Y axis
  //       p.style.transform =
  //         "translate3d(0," +
  //         (0 - p.getAttribute("offset") + parseInt(p.getAttribute("calc"))) * p.getAttribute("speed") +
  //         "px, 0)";
  //       //sets Y - needed to be individually called in scroll listener - also checks if y is already set
  //       if (p.getAttribute("y") == null) p.setAttribute("y", "");
  //     }
  //   });

  //   //needed to set direction (best solution for now)
  //   let parX = document.querySelectorAll(".parallax[x]");
  //   let parY = document.querySelectorAll(".parallax[y]");

  //   //scroll listener
  //   window.addEventListener("scroll", function () {
  //     posY = window.scrollY;

  //     //this is calculated whenever the user scrolls (suprisingly performant)
  //     parX.forEach(function (p) {
  //       p.style.transform =
  //         "translate3d(" +
  //         (posY - p.getAttribute("offset") + parseInt(p.getAttribute("calc"))) * p.getAttribute("speed") +
  //         "px, 0, 0)";
  //     });
  //     parY.forEach(function (p) {
  //       p.style.transform =
  //         "translate3d(0," +
  //         (posY - p.getAttribute("offset") + parseInt(p.getAttribute("calc"))) * p.getAttribute("speed") +
  //         "px, 0)";
  //     });
  //   });
  // });
</script>

<div class="top-fade" />
<div class="container">
  <slot />
</div>

<style lang="scss">
  .top-fade {
    position: absolute;
    z-index: -1;
    top: 0;
    left: 0;
    width: 100%;
    height: 30rem;
    background: linear-gradient(0deg, rgba(255, 255, 255, 0) 0%, rgba(255, 255, 255, 1) 100%);
  }
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
