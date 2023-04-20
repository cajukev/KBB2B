<script>
  import { getContext, onMount } from "svelte";
  import translation from "./index.json";
  const lang = getContext("lang");
  let t = translation[lang];
  let firstScroll = false;

  let section2;
  let section5;
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

    // Random number between 1 and 9
    currentSquare = Math.floor(Math.random() * 9);
    let squareElem = section5.getElementsByClassName("squares")[0].getElementsByTagName("div")[currentSquare];

    // First scroll event listener
    window.addEventListener("scroll", function () {
      if (firstScroll) return;
      firstScroll = true;
      section2.classList.remove("hiddenStart");
      scrollArrow.classList.add("remove");

      squareElem.classList.add("special");

      let listItems = section2.getElementsByTagName("li");
      for (let i = 0; i < listItems.length; i++) {
        listItems[i].style.animation = "fade-up 1s ease-in-out forwards";
        listItems[i].style.animationDelay = `${i * 0.25 + 0.25}s`;
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
    if (index === currentSquare) return;
    let timePassed = new Date().getTime() - timeStart;

    document
      .getElementById("section5")
      .getElementsByClassName("squares")[0]
      .getElementsByTagName("div")
      [currentSquare].classList.remove("special");
    document
      .getElementById("section5")
      .getElementsByClassName("squares")[0]
      .getElementsByTagName("div")
      [index].classList.add("special");
    document.getElementById("section5").getElementsByClassName("squares")[0].getElementsByTagName("div")[
      index
    ].style.animationDelay = `${(-timePassed / 1000) % 6}s`;
    currentSquare = index;
  };

  let siteHover = (i) => {
    const content = document.getElementById("section3").getElementsByClassName("content")[i];
    content.getElementsByClassName("bg")[0].style.opacity = 1;
    content.getElementsByClassName("bg")[0].style.transform = "scaleX(1)";
    content.getElementsByClassName("text")[0].style.opacity = 1;
    content.getElementsByClassName("text")[0].style.transform = "translateY(0)";
    content.getElementsByClassName("left")[0].style.opacity = 1;
    content.getElementsByClassName("left")[0].style.transform = "translateX(0)";
    content.getElementsByClassName("right")[0].style.opacity = 1;
    content.getElementsByClassName("right")[0].style.transform = "translateX(0)";
  };
  let siteLeave = (i) => {
    const content = document.getElementById("section3").getElementsByClassName("content")[i];
    content.getElementsByClassName("left")[0].style.opacity = 0;
    content.getElementsByClassName("left")[0].style.transform = "translateX(-1rem)";
    content.getElementsByClassName("text")[0].style.opacity = 0;
    content.getElementsByClassName("text")[0].style.transform = "translateY(1rem)";
    content.getElementsByClassName("right")[0].style.opacity = 0;
    content.getElementsByClassName("right")[0].style.transform = "translateX(1rem)";
    content.getElementsByClassName("bg")[0].style.opacity = 0;
    content.getElementsByClassName("bg")[0].style.transform = "scaleX(0.75)";
  };
</script>

<div class="section" id="section1">
  <div class="squares">
    <div class="sq1 bp2" />
    <div class="sq2" />
    <div class="sq3 special" />
    <div class="sq4" />
    <div class="sq5 bp2" />
    <span class="light" />
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
    <p class="title"><b>{t["section2.title"]}</b></p>
    <i>{t["section2.hook"]}</i>
    <p>{@html t["section2.list"]}</p>
  </div>
</div>

<div class="section" id="section3">
  <h2 class="appear">{t["section3.title"]}</h2>
  <div class="frame appear" id="site4">
    <picture>
      <source srcset="site-4-400.webp" media="(max-width: 450px)" type="image/webp" />
      <source srcset="site-4-400.jpg" media="(max-width: 450px)" type="image/jpeg" />
      <source srcset="site-4-800.webp" media="(max-width: 800px)" type="image/webp" />
      <source srcset="site-4-800.jpg" media="(max-width: 800px)" type="image/jpeg" />
      <source srcset="site-4-1200.webp" media="(max-width: 1200px)" type="image/webp" />
      <source srcset="site-4-1200.jpg" media="(max-width: 1200px)" type="image/jpeg" />
      <source srcset="site-4-1600.webp" type="image/webp" />
      <img src="site-4-1600.jpg" alt="site1" loading="lazy" class="preview" />
    </picture>
    <div
      class="content"
      on:pointerdown={() => siteHover(0)}
      on:pointerover={() => siteHover(0)}
      on:mouseleave={() => siteLeave(0)}
    >
      <div class="bg" />
      <div class="left">
        <img src="/Logos/svelte.svg" alt="" />
      </div>
      <div class="text">
        <p class="title">{t["section3.4.title"]}</p>
        <div class="links">
          <a href={t["section3.4.link"]} >
            {t["section3.4.link"]}
          </a>
          <a href={t["section3.4.github"]} >
            <img src="Github.svg" alt={t["section3.4.title"] + " github"} />
          </a>
        </div>

        <p class="textContent">{@html t["section3.4.content"]}</p>
        <div class="mobileImages ubp2">
          <img src="/Logos/svelte.svg" alt="" />
          <img src="/Logos/supabase.svg" alt="" />
        </div>
      </div>
      <div class="right">
        <img src="/Logos/supabase.svg" alt="" />
      </div>
    </div>
  </div>

  <div class="frame appear" id="site1">
    <!-- Responsive picture images with webp option -->
    <picture>
      <source srcset="site-1-400.webp" media="(max-width: 450px)" type="image/webp" />
      <source srcset="site-1-400.jpg" media="(max-width: 450px)" type="image/jpeg" />
      <source srcset="site-1-800.webp" media="(max-width: 800px)" type="image/webp" />
      <source srcset="site-1-800.jpg" media="(max-width: 800px)" type="image/jpeg" />
      <source srcset="site-1-1200.webp" media="(max-width: 1200px)" type="image/webp" />
      <source srcset="site-1-1200.jpg" media="(max-width: 1200px)" type="image/jpeg" />
      <source srcset="site-1-1600.webp" type="image/webp" />
      <img src="site-1-1600.jpg" alt="site1" loading="lazy" class="preview" />
    </picture>
    <div
      class="content"
      on:pointerdown={() => siteHover(1)}
      on:pointerover={() => siteHover(1)}
      on:mouseleave={() => siteLeave(1)}
    >
      <div class="bg" />
      <div class="left">
        <img src="/Logos/svelte.svg" alt="" />
      </div>
      <div class="text">
        <p class="title">{t["section3.1.title"]}</p>
        <div class="links">
          <a href={t["section3.1.link"]}>
            {t["section3.1.link"]}
          </a>
          <a href={t["section3.1.github"]} >
            <img src="Github.svg" alt={t["section3.1.title"] + " github"} />
          </a>
        </div>
        <p class="textContent">{@html t["section3.1.content"]}</p>
        <div class="mobileImages ubp2">
          <img src="/Logos/svelte.svg" alt="" />
          <img src="/Logos/supabase.svg" alt="" />
        </div>
      </div>
      <div class="right">
        <img src="/Logos/supabase.svg" alt="" />
      </div>
    </div>
  </div>

  <div class="frame appear" id="site3">
    <picture>
      <source srcset="site-3-400.webp" media="(max-width: 450px)" type="image/webp" />
      <source srcset="site-3-400.jpg" media="(max-width: 450px)" type="image/jpeg" />
      <source srcset="site-3-800.webp" media="(max-width: 800px)" type="image/webp" />
      <source srcset="site-3-800.jpg" media="(max-width: 800px)" type="image/jpeg" />
      <source srcset="site-3-1200.webp" media="(max-width: 1200px)" type="image/webp" />
      <source srcset="site-3-1200.jpg" media="(max-width: 1200px)" type="image/jpeg" />
      <source srcset="site-3-1600.webp" type="image/webp" />
      <img src="site-3-1600.jpg" alt="site3" loading="lazy" class="preview" />
    </picture>
    <div
      class="content"
      on:pointerdown={() => siteHover(2)}
      on:pointerover={() => siteHover(2)}
      on:mouseleave={() => siteLeave(2)}
    >
      <div class="bg" />
      <div class="left">
        <img src="/Logos/react.svg" alt="" />
      </div>
      <div class="text">
        <p class="title">{t["section3.3.title"]}</p>
        <div class="links">
          <a href={t["section3.3.link"]}>
            {t["section3.3.link"]}
          </a>
          <a href={t["section3.3.github"]} >
            <img src="Github.svg" alt={t["section3.3.title"] + " github"} />
          </a>
        </div>
        <p class="textContent">{@html t["section3.3.content"]}</p>
        <div class="mobileImages ubp2">
          <img src="/Logos/react.svg" alt="" />
          <img src="/Logos/spring.svg" alt="" />
        </div>
      </div>
      <div class="right">
        <img src="/Logos/spring.svg" alt="" />
      </div>
    </div>
  </div>
  <div class="frame appear" id="site2">
    <picture>
      <source srcset="site-2-400.webp" media="(max-width: 450px)" type="image/webp" />
      <source srcset="site-2-400.jpg" media="(max-width: 450px)" type="image/jpeg" />
      <source srcset="site-2-800.webp" media="(max-width: 800px)" type="image/webp" />
      <source srcset="site-2-800.jpg" media="(max-width: 800px)" type="image/jpeg" />
      <source srcset="site-2-1200.webp" media="(max-width: 1200px)" type="image/webp" />
      <source srcset="site-2-1200.jpg" media="(max-width: 1200px)" type="image/jpeg" />
      <source srcset="site-2-1600.webp" type="image/webp" />
      <img src="site-2-1600.jpg" alt="site1" loading="lazy" class="preview" />
    </picture>
    <div
      class="content"
      on:pointerdown={() => siteHover(3)}
      on:pointerover={() => siteHover(3)}
      on:mouseleave={() => siteLeave(3)}
    >
      <div class="bg" />
      <div class="left">
        <img src="/Logos/vue.svg" alt="" />
      </div>
      <div class="text">
        <p class="title">{t["section3.2.title"]}</p>
        <div class="links">
          <a href={t["section3.2.link"]} >
            {t["section3.2.link"]}
          </a>
          <a href={t["section3.2.github"]} >
            <img src="Github.svg" alt={t["section3.2.title"] + " github"} />
          </a>
        </div>

        <p class="textContent">{@html t["section3.2.content"]}</p>
        <div class="mobileImages ubp2">
          <img src="/Logos/vue.svg" alt="" />
          <img src="/Logos/css.svg" alt="" />
        </div>
      </div>
      <div class="right">
        <img src="/Logos/css.svg" alt="" />
      </div>
    </div>
  </div>
</div>
<div class="section" id="section4">
  <h2 class="appear">{t["section4.title"]}</h2>
  <div class="content">
    <div class="frame appear">
      <h2>{t["section4.1.title"]}</h2>
      <p class="description">{@html t["section4.1.description"]}</p>
      <p class="learned"><i>{t["section4.learned"]}</i></p>
      <p>{@html t["section4.1.content"]}</p>
    </div>
    <div class="frame frame-special appear">
      <h2>{t["section4.2.title"]}</h2>
      <p class="description">{@html t["section4.2.description"]}</p>
      <p class="learned"><i>{t["section4.learned"]}</i></p>
      <p>{@html t["section4.2.content"]}</p>
    </div>
    <div class="frame appear">
      <h2>{t["section4.3.title"]}</h2>
      <p class="description">{@html t["section4.3.description"]}</p>
      <p class="learned"><i>{t["section4.learned"]}</i></p>
      <p>{@html t["section4.3.content"]}</p>
    </div>
    <div class="frame frame-special appear">
      <h2>{t["section4.4.title"]}</h2>
      <p class="description">{@html t["section4.4.description"]}</p>
      <p class="learned"><i>{t["section4.learned"]}</i></p>
      <p>{@html t["section4.4.content"]}</p>
    </div>
  </div>
</div>

<div class="section" id="section5" bind:this={section5}>
  <div class="flavour">
    <div class="squares appear">
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
    <i class="appear">{t["section5.quote"]}</i>
  </div>
</div>

<div class="section" id="section6">
  <h2 class="appear">{t["section6.title"]}</h2>
  <div class="contactButtons">
    <a href="mailto:kevin.brousseau98@gmail.com" class="appear" >
      <img src="at.svg" alt="email">
    </a>
    <a href="tel:514-550-7051" class="appear" >
      <img src="Phone.svg" alt="phone">
    </a>
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
      opacity: 0.01;
      animation: fly-up-separator 1.25s 2s cubic-bezier(0, 0.68, 0.59, 1.01) forwards;
    }
  }
  #section1 {
    .squares {
      position: relative;
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
      & .light {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        transform: scaleX(2) scaleY(1.3);
        background: linear-gradient(
          180deg,
          rgba(255, 255, 255, 0) 0%,
          rgba(255, 255, 255, 0.615306) 23.96%,
          rgba(255, 255, 255, 0.67) 51.04%,
          rgba(255, 255, 255, 0.625824) 71.87%,
          rgba(255, 255, 255, 0) 98.44%
        );
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
        opacity: 0.01;
        animation-delay: 2s;
      }
    }
    & .scrollArrow {
      position: absolute;
      bottom: -6rem;
      left: 50%;
      transform: translateX(-50%);
      width: 2rem;
      height: 2rem;
      opacity: 0.01;
      animation: fly-up-arrow 1.25s 2s cubic-bezier(0, 0.68, 0.59, 1.01) forwards,
        arrow-bounce 2s 3.25s ease-in-out infinite;
    }
  }
  #section2 {
    padding: 2rem;
    @media screen and (max-width: $breakpoint-3-1) {
      padding: 3rem 1rem;
    }
    @media screen and (max-width: $breakpoint-2-1) {
      padding: 3rem 0.5rem;
    }
    &.hiddenStart {
      opacity: 0.01;
    }
    &:not(.hiddenStart) {
      opacity: 1;
      transform-origin: center;

      & .frame {
        animation: fly-up 1s 0s cubic-bezier(0, 0.68, 0.59, 1.01) forwards;
      }
    }
    display: grid;
    place-content: center;
    & .frame {
      padding: 1.5rem;
      width: fit-content;
      max-width: 100%;
      justify-self: center;
      & p {
        max-width: 30rem;
      }
      & .title {
        margin-bottom: 1.25rem;
      }
      & p {
        margin-top: 0.75rem;
      }
      & li {
        margin-top: 0.25rem;
      }
    }
  }
  #section3 {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 2rem 0rem;
    @media screen and (max-width: $breakpoint-4-1) {
      grid-template-columns: 1fr;
      padding: 3rem 0rem;
    }
    & .frame {
      width: 100%;
      margin-top: 4rem;
      position: relative;
      & img.preview {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: center;
      }
      & .content {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;

        & .bg {
          position: absolute;
          width: 100%;
          height: 100%;
          background: linear-gradient(
            90deg,
            rgba($bg1, 0.8) 0%,
            rgba($bg1, 0.9) 10%,
            rgba($bg1, 1) 50%,
            rgba($bg1, 0.9) 90%,
            rgba($bg1, 0.8) 100%
          );
          z-index: 0;
          opacity: 0.01;
          transition: all 0.25s ease;
        }
        & .mobileImages {
          @media screen and (max-width: $breakpoint-2-1) {
            display: flex;
            justify-content: space-around;
          }
          & img {
            filter: drop-shadow(0px 0.25rem 0.5rem rgba(0, 0, 0, 0.3));
            height: clamp(10rem, 20vw, 18rem);
            padding: 1rem;
          }
        }
        & .left,
        .right {
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          @media screen and (max-width: $breakpoint-2-1) {
            display: none;
          }
          & img {
            filter: drop-shadow(0px 0.25rem 0.5rem rgba(0, 0, 0, 0.3));
            height: clamp(10rem, 20vw, 18rem);
            padding: 1rem;
          }
        }
        & .text {
          text-align: center;
          max-width: 35rem;
          margin: 0rem 0.5rem;
          @media screen and (max-width: $breakpoint-3-1) {
            margin: 0rem 1rem;
          }
          & .title {
            margin-top: 1.5rem;
            font-size: $subheader-font-size;
            font-weight: 700;
          }
          & .links {
            margin-top: 0.25rem;
            display: flex;
            align-items: center;
            justify-content: center;
            & .siteLink {
            }
            & img {
              width: 1.5rem;
              height: 1.5rem;
              margin-left: 1rem;
            }
          }
          & .textContent {
            margin-top: 1.5rem;
            margin-bottom: 1.5rem;
          }
        }
        & .left,
        .text,
        .right {
          z-index: 1;
          opacity: 0.01;
          transition: all 0.25s ease;
        }
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
    display: flex;
    flex-direction: column;
    align-items: center;
    & .content {
      margin-top: 4rem;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.5rem;
      justify-items: center;
      width: fit-content;
      @media screen and (max-width: $breakpoint-2-1) {
        grid-template-columns: 1fr;
      }
      & .frame {
        width: 100%;
        max-width: 35rem;
        height: 100%;
        padding: 2rem 1rem;
        & h2 {
          margin-bottom: 1.5rem;
        }
        & .learned {
          margin-bottom: 0.5rem;
        }
        & .description {
          margin-bottom: 1.5rem;
        }
      }
    }
  }

  #section5 {
    padding: 2rem;
    @media screen and (max-width: $breakpoint-3-1) {
      padding: 3rem 1rem;
    }
    @media screen and (max-width: $breakpoint-2-1) {
      padding: 3rem 0.5rem;
    }
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
  }

  #section6 {
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

    & .contactButtons {
      display: flex;
      justify-content: center;
      width: 100%;
      margin-top: 2rem;
      gap: 4rem;
      & a {
        padding: 1.5rem;
        background: linear-gradient(135deg, #ff9c41 0%, #f27c0f 100%);
        box-shadow: 0px 0px 3rem rgba(233, 111, 0, 0.6);
        border-radius: 25%;
        & img {
          width: 3rem;
          height: 3rem;
        }
      }
    }
  }
</style>
