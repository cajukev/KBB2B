<script>
  import { getContext, onMount } from "svelte";
  import translation from "./index.json";
  const lang = getContext("lang");
  let t = translation[lang];
  let firstScroll = false;

  let section2;
  let scrollArrow;

  let currentSquare = 0;

  // Get current ms
  let timeStart = new Date().getTime();


  onMount(() => {
    // Section 1 Stagger
    let titleWords = document.getElementById("section1").getElementsByClassName("stagger");
    for (let i = 0; i < titleWords.length; i++) {
      titleWords[i].style.animationDelay = `${i * 0.25}s`;
    }
    // Extra delay for the last word
    titleWords[titleWords.length - 1].style.animationDelay = `${(titleWords.length - 1) * 0.25 + 0.25}s`;
    // First scroll event listener
    window.addEventListener("scroll", function () {
      if (firstScroll) return;
      firstScroll = true;
      section2.classList.remove("hiddenStart");
      scrollArrow.classList.add("animOut");
      // Random number between 1 and 9
      currentSquare = Math.floor(Math.random() * 9);
      let squareElem = section2.getElementsByClassName("squares")[0].getElementsByTagName("div")[currentSquare];

      squareElem.classList.add("special");
      // Section 2 Stagger
      let section2ListItems = section2.getElementsByTagName("li");
      for (let i = 0; i < section2ListItems.length; i++) {
        section2ListItems[i].classList.add("stagger");
        section2ListItems[i].style.animationName = "fade-down";
        section2ListItems[i].style.animationDelay = `${i * 0.5 + 0.5}s`;
      }
    });

    // Elements appear when scrolled into view Event Listener
    const appear = document.querySelectorAll(".appear, .appear2");
    const cb = function (entries) {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("inview");
        }
      });
    };
    const io = new IntersectionObserver(cb, { threshold: 0.33 });
    appear.forEach((el) => {
      io.observe(el);
    });
  });

  let onSqClick = (index) => {
    if(index === currentSquare) return;
    let timePassed = new Date().getTime() - timeStart;

    document
      .getElementById("section2")
      .getElementsByClassName("squares")[0]
      .getElementsByTagName("div")
      [currentSquare].classList.remove("special");
    document
      .getElementById("section2")
      .getElementsByClassName("squares")[0]
      .getElementsByTagName("div")
      [index].classList.add("special");
      document
      .getElementById("section2")
      .getElementsByClassName("squares")[0]
      .getElementsByTagName("div")
      [index].style.animationDelay = `${-timePassed/1000%6}s`;
    currentSquare = index;
  };
</script>

<div class="section" id="section1">
  <div class="squares">
    <div class="sq1 bp2" />
    <div class="sq2" />
    <div class="sq3 special" />
    <div class="sq4" />
    <div class="sq5 bp2" />
  </div>
  <div class="content">
    <h1>{@html t.title}</h1>
    <p class="fly-up">{@html t.description}</p>
  </div>
  <svg bind:this={scrollArrow} class="scrollArrow" viewBox="0 0 10 12" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path
      fill-rule="evenodd"
      clip-rule="evenodd"
      d="M9.84308 6.21093C9.95108 6.06993 10.0001 5.91093 10.0001 5.75493C10.0001 5.36593 9.69408 4.99993 9.25108 4.99993H7.30482L9.843 1.711C9.951 1.57 10 1.411 10 1.255C10 0.866 9.694 0.5 9.251 0.5H0.75C0.305 0.5 0 0.867 0 1.255C0 1.412 0.0499999 1.571 0.159 1.712C0.848412 2.60256 1.81566 3.85143 2.70523 4.99993H0.750077C0.305077 4.99993 7.73668e-05 5.36693 7.73668e-05 5.75493C7.73668e-05 5.91193 0.0500773 6.07093 0.159077 6.21193C1.36208 7.76593 3.41108 10.4109 4.41708 11.7099C4.55908 11.8939 4.77708 11.9999 5.00908 11.9999C5.23908 11.9999 5.45808 11.8929 5.60008 11.7089L9.84308 6.21093ZM7.72508 6.49993H2.27908L5.00708 10.0219L7.72508 6.49993ZM7.725 2H2.279L4.60263 4.99993H5.40989L7.725 2Z"
      fill="#A6AEF2"
    />
  </svg>
</div>

<div class="section hiddenStart" id="section2" bind:this={section2}>
  <div class="frame">
    <h3>{t["section2.title"]}</h3>
    <i>{t["hook"]}</i>
    <p>{@html t["section2.list"]}</p>
  </div>
  <div class="flavour">
    <div class="squares">
      <div class="sq1" on:click={() => onSqClick(0)} />
      <div class="sq2" on:click={() => onSqClick(1)} />
      <div class="sq3" on:click={() => onSqClick(2)} />
      <div class="sq4" on:click={() => onSqClick(3)} />
      <div class="sq5" on:click={() => onSqClick(4)} />
      <div class="sq6" on:click={() => onSqClick(5)} />
      <div class="sq7" on:click={() => onSqClick(6)} />
      <div class="sq8" on:click={() => onSqClick(7)} />
      <div class="sq9" on:click={() => onSqClick(8)} />
    </div>
    <i>{t["section2.quote"]}</i>
    <!-- <div class="img-wrapper">
      <img src="site-1-400.jpg" alt="img1" class="img1" />
      <img src="site-2-400.jpg" alt="img2" class="img2" />
      <img src="site-3-400.jpg" alt="img3" class="img3" />
    </div> -->
  </div>
</div>

<div class="section" id="section3">
  <div class="frame appear2">
    <h1>{t["section3.heading"]}</h1>
    <svg viewBox="0 0 330 103" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path
        d="M1.50012 72.7099V74.2099H3.00012H39.2876H40.7876V72.7099V54.1313V52.6313H39.2876H3.00012H1.50012V54.1313V72.7099ZM85.8209 38.1722L86.5246 28.3137H116.971H118.471V26.8137V5.32216V3.82216H116.971H63.236H61.8293L61.7391 5.22595L58.1568 60.9616L58.0292 62.9468L59.9729 62.5235C64.8077 61.4704 70.0729 60.9395 75.7741 60.9395C81.4697 60.9395 85.4946 61.7861 88.049 63.2892C90.4797 64.7196 91.6549 66.7802 91.6549 69.6674C91.6549 72.3413 90.6237 74.3083 88.5261 75.7446L88.5159 75.7517C86.4022 77.2249 83.0912 78.0717 78.3613 78.0717C71.2198 78.0717 64.4471 76.405 58.0243 73.0624L55.8318 71.9213V74.393V95.8845V96.9312L56.8142 97.2924C59.9915 98.4607 63.9888 99.4079 68.7777 100.15L68.7798 100.151C73.6239 100.895 78.2777 101.269 82.7397 101.269C90.7888 101.269 97.8544 99.8343 103.892 96.9109C109.918 93.9932 114.57 89.9014 117.786 84.6258C121.036 79.3244 122.65 73.3536 122.65 66.7544C122.65 58.0015 119.899 50.9282 114.278 45.7185C108.664 40.472 100.983 37.9368 91.4301 37.9368C89.6484 37.9368 87.7785 38.0156 85.8209 38.1722ZM142.328 89.2387L142.331 89.2429C148.745 97.3288 158.367 101.269 170.904 101.269C183.235 101.269 192.783 96.9648 199.304 88.2387C205.813 79.545 208.957 67.0459 208.957 50.9594C208.957 35.0882 206.031 22.9124 199.94 14.6744C193.786 6.3518 184.568 2.26855 172.563 2.26855C159.849 2.26855 149.935 6.58219 143.057 15.3353L143.054 15.3392C136.264 24.0377 132.984 36.6133 132.984 52.8367C132.984 68.9837 136.008 81.2124 142.328 89.2387ZM283.418 94.0991L283.424 94.1045C288.088 98.7021 294.241 100.945 301.725 100.945C309.778 100.945 316.339 98.5537 321.23 93.6415C326.113 88.7383 328.5 82.2693 328.5 74.393C328.5 66.7564 326.281 60.6101 321.676 56.1622C317.129 51.7264 310.945 49.5889 303.317 49.5889C295.353 49.5889 288.824 51.9575 283.894 56.8123C278.949 61.6371 276.542 68.1398 276.542 76.1408C276.542 83.464 278.802 89.5013 283.418 94.0991ZM306.585 68.8882L306.59 68.8953L306.595 68.9024C307.552 70.2869 308.119 72.3419 308.119 75.2345C308.119 78.2151 307.529 80.2999 306.543 81.6751L306.535 81.6855L306.528 81.6961C305.644 82.9739 304.384 83.6388 302.521 83.6388C300.705 83.6388 299.496 82.9972 298.659 81.7791C297.778 80.4316 297.254 78.432 297.254 75.6229C297.254 72.6488 297.842 70.5012 298.85 69.0247L298.855 69.018L298.86 69.0111C299.81 67.5876 301.102 66.895 302.919 66.895C304.483 66.895 305.661 67.5187 306.585 68.8882ZM252.767 99.6502H253.575L254.019 98.9755L315.184 6.14747L316.716 3.82216H313.931H293.035H292.227L291.782 4.49686L230.617 97.3249L229.085 99.6502H231.87H252.767ZM223.912 47.1672L223.918 47.1726C228.582 51.7702 234.735 54.013 242.219 54.013C250.14 54.013 256.627 51.6654 261.515 46.8492C266.454 41.9832 268.861 35.4625 268.861 27.4611C268.861 19.9039 266.638 13.7685 262.049 9.24188C257.503 4.71369 251.315 2.52749 243.678 2.52749C235.58 2.52749 229.013 4.94112 224.163 9.90579L224.163 9.90604C219.372 14.8125 217.036 21.3006 217.036 29.2089C217.036 36.5321 219.295 42.5694 223.912 47.1672ZM246.917 21.9121L246.929 21.9306L246.941 21.9488C247.87 23.3431 248.414 25.3683 248.414 28.1731C248.414 31.2589 247.839 33.391 246.889 34.7642C245.993 36.0604 244.765 36.7069 243.015 36.7069C241.198 36.7069 239.99 36.0653 239.153 34.8471C238.272 33.4997 237.748 31.5001 237.748 28.691C237.748 25.7169 238.336 23.5693 239.344 22.0928L239.349 22.0861L239.353 22.0793C240.303 20.6557 241.596 19.9631 243.413 19.9631C244.972 19.9631 246.084 20.5793 246.917 21.9121ZM176.168 30.9364C177.429 35.1184 178.095 41.7542 178.095 50.9594C178.095 60.4579 177.369 67.4312 175.987 71.9827C174.573 76.6385 172.724 78.0717 170.904 78.0717C169.878 78.0717 168.994 77.7005 168.186 76.8631C167.34 75.9864 166.553 74.5676 165.905 72.4705C164.644 68.1971 163.979 61.4947 163.979 52.2541C163.979 42.8423 164.705 35.9353 166.086 31.4286C167.497 26.8244 169.342 25.4007 171.17 25.4007C172.23 25.4007 173.125 25.7755 173.926 26.5983C174.768 27.4617 175.543 28.8611 176.168 30.9364Z"
        fill="#FF7C05"
        stroke="white"
        stroke-width="3"
      />
    </svg>
    <p>{t["section3.description"]}</p>
  </div>
</div>

<div class="section" id="section4">
  <div class="options">
    <div class="frame appear" id="option1">
      <p class="name"><b>{@html t["section4.1.name"]}</b></p>
      <p class="price">{@html t["section4.1.price"]}</p>
      <p class="description">{@html t["section4.1.description"]}</p>
      <button class="btn btn-secondary">{t["cta"]}</button>
    </div>
    <div class="frame appear" id="option2">
      <p class="name"><b>{@html t["section4.2.name"]}</b></p>
      <p class="price">{@html t["section4.2.price"]}</p>
      <p class="description">{@html t["section4.2.description"]}</p>
      <button class="btn btn-secondary">{t["cta"]}</button>
    </div>
    <div class="frame-special appear" id="option3">
      <p class="name"><b>{@html t["section4.3.name"]}</b></p>
      <p class="price">{@html t["section4.3.price"]}</p>
      <p class="description">{@html t["section4.3.description"]}</p>
      <button class="btn btn-primary">{t["cta"]}</button>
    </div>
    <div class="frame appear" id="option4">
      <p class="name"><b>{t["section4.4.name"]}</b></p>
      <p class="price">{t["section4.4.price"]}</p>
      <button class="btn btn-secondary">{t["cta"]}</button>
    </div>
  </div>
</div>

<div class="section" id="section5">
    <h2 class="appear">{t["section5.title"]}</h2>
    <form class="appear">
      <label for="name">{t["section5.form.name.label"]}</label>
      <input type="text" id="name" name="name" placeholder="{t["section5.form.name.placeholder"]}" required>
      <label for="comm">{t["section5.form.comm.label"]}</label>
      <input type="email" id="comm" name="comm" placeholder="{t["section5.form.comm.placeholder"]}" required>
      <label for="message">{t["section5.form.times.label"]}</label>
      <textarea id="message" name="message" placeholder="{t["section5.form.times.placeholder"]}" required></textarea>
    </form>
    <button class="btn btn-primary appear">{t["send"]}</button>
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
      animation: fly-up-separator 1.75s 2s cubic-bezier(0, 0.68, 0.59, 1.01) forwards;
    }
  }
  #section1 {
    .squares {
      margin-top: 2rem;
      display: flex;
      align-items: center;
      justify-content: space-around;
      & div {
        &.special {
          height: clamp(7rem, 20vw, 18rem);
          width: clamp(7rem, 20vw, 18rem);
          animation-duration: 12s;
          animation-delay: 2s;
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
        animation-delay: 1.75s;
      }
    }
    & .scrollArrow {
      position: absolute;
      bottom: -6rem;
      left: 50%;
      transform: translateX(-50%);
      width: 2rem;
      height: 2rem;
      opacity: 0;
      animation: fly-up-arrow 1.5s 2.5s cubic-bezier(0.25, 1, 0.5, 1) forwards,
        arrow-bounce 2s 3.75s ease-in-out infinite;
    }
  }

  #section2 {
    & .flavour {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.5rem;
      justify-self: center;
      & .squares {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(3, 1fr);
        gap: 1rem;
        & div {
          cursor: pointer;
          transition: transform 0.25s ease;
          &:hover:not(.special) {
            transform: scale(1.1);
          }
        }
        & .special {
          animation-duration: 6s;
          animation-delay: 0s;
        }
      }
      & i {
        margin-top: 1.5rem;
      }
    }
    &.hiddenStart {
      opacity: 0;
    }
    &:not(.hiddenStart) {
      opacity: 1;
      transform-origin: center;

      & .frame, .flavour {
        animation: fly-up 1s 0s cubic-bezier(0, 0.68, 0.59, 1.01) forwards;
      }
    }
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
    padding: 2rem;
    @media screen and (max-width: $breakpoint-4-1) {
      grid-template-columns: 1fr;
      padding: 3rem 0rem;
    }
    & .frame {
      padding: 1.5rem;
      width: fit-content;
      max-width: 100%;
      justify-self: center;
      & h3,
      p {
        max-width: 30rem;
      }
      & h3 {
        margin-bottom: 1.25rem;
      }
      & p {
        margin-top: 0.75rem;
      }
      & li {
        margin-top: 0.25rem;
      }
    }
    & .images {
      place-self: center;
      & .img-wrapper {
        display: flex;
        gap: 1rem;
        margin-top: 1rem;
        @media screen and (max-width: $breakpoint-2-1) {
          flex-direction: column;
        }
        & img {
          width: clamp(10rem, 11vw, 14.5rem);

          aspect-ratio: 2/3;
          object-fit: cover;
          border: 1px solid $accent-color2;
          box-shadow: $box-shadow-1;
          @media screen and (max-width: $breakpoint-2-1) {
            width: 100%;
            aspect-ratio: 1/1;
          }
        }
      }
    }
  }
  #section3 {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    @media screen and (max-width: $breakpoint-3-1) {
      padding: 3rem 1rem;
    }
    @media screen and (max-width: $breakpoint-2-1) {
      padding: 3rem 0.5rem;
    }
    .frame {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      width: fit-content;
      gap: 0.75rem;
      padding: 1rem 3rem;
      border: 0.25rem solid $accent-color2;
      & h1 {
        font-weight: 700;
        text-align: center;
      }
      & svg {
        width: 100%;
        max-width: 15rem;
        filter: drop-shadow(0px 0.125rem 0.15rem rgba(60, 62, 79, 0.19));
      }
      & p {
        text-align: center;
        max-width: 30rem;
        line-height: 1.5;
      }
    }
  }
  #section4 {
    padding: 2rem;
    @media screen and (max-width: $breakpoint-3-1) {
      padding: 3rem 1rem;
    }
    @media screen and (max-width: $breakpoint-2-1) {
      padding: 3rem 0.5rem;
    }
    & .options {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: auto auto;
      gap: 1rem;
      justify-items: center;
      @media screen and (max-width: $breakpoint-3-1) {
        display: flex;
        flex-direction: column;
        align-items: center;
      }
      & #option1 {
        grid-column: 1 / 2;
        grid-row: 1 / 2;
      }
      & #option2 {
        grid-column: 3 / 4;
        grid-row: 1 / 3;
      }
      & #option3 {
        grid-column: 2 / 3;
        grid-row: 1 / 3;
      }
      & #option4 {
        grid-column: 1 / 2;
        grid-row: 2 / 3;
      }
    }
    & .frame,
    .frame-special {
      padding: 1rem;
      max-width: 25rem;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      height: fit-content;
      & .name{
        line-height: 1.5;
      }
      & .price{
        margin-top: 0.5rem;
      }
      & .description, button{
        margin-top: 1rem;
      }
    }
  }

  #section5{
    padding: 2rem;
    @media screen and (max-width: $breakpoint-3-1) {
      padding: 3rem 1rem;
    }
    @media screen and (max-width: $breakpoint-2-1) {
      padding: 3rem 0.5rem;
    }
    display: flex;
    flex-direction: column;
    align-items: center;
    & h2{
      font-weight: 300;
    }
    & form{
      margin-top: 3rem;
      display: flex;
      flex-direction: column;
      max-width: 35rem;
      width: 100%;
      & label{
        margin-top: 1rem;
        margin-bottom: 0.25rem;
      }
      & textArea{
        resize: none;
        height: 10rem;
      }
    }
    & button{
      margin-top: 1rem;
      width: fit-content;
    }
  }
</style>
