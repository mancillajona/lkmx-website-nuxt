<template>
  <simple-layout :id="[`body-${$store.state.class.bodyClass}`]">
    <template #header>
      <simple-header mode="full">
        <a v-on:click.prevent="toggleMenu" class="header__menu" href="">
          <img
            class="menu-open"
            src="~/assets/img/icon-menu-open.svg"
            alt="Menu"
          />
          <img
            class="menu-close"
            src="~/assets/img/icon-menu-close.svg"
            alt="Menu"
          />
        </a>
        <div class="logo-container">
          <img
            class="header__logo"
            src="~/assets/img/lkmx-logo.svg"
            alt="LKMX"
          />
        </div>
        <nav class="main-navigation">
          <nuxt-link to="/">Servicios</nuxt-link>
          <nuxt-link to="/">Metodología</nuxt-link>
          <nuxt-link to="/">Equipo</nuxt-link>
          <nuxt-link to="/">Tecnología</nuxt-link>
          <nuxt-link to="/">Contacto</nuxt-link>
        </nav>
      </simple-header>
    </template>
    <Nuxt />
    <template #footer>
      <simple-footer class="">
        <section class="simple-footer__content">
          <div class="simple-footer__content__brand">
            <img
              src="~/assets/img/lkmx-logo-full-dark.svg"
              alt="Stake or Die!"
            />
            <p>
              Prestamos servicios de ingeniería de software con nuestra sede en
              Ensenada, Baja California, México desde enero de 2014.
            </p>
            <div class="social">
              <nuxt-link to="/">
                <img src="~/assets/img/icon-social-instagram.svg" alt="" />
              </nuxt-link>
              <nuxt-link to="/">
                <img src="~/assets/img/icon-social-twitter.svg" alt="" />
              </nuxt-link>
              <nuxt-link to="/">
                <img src="~/assets/img/icon-social-linkedin.svg" alt="" />
              </nuxt-link>
            </div>
          </div>
          <nav class="simple-footer__content__navigation">
            <nuxt-link to="/">Servicios</nuxt-link>
            <nuxt-link to="/">Metodología</nuxt-link>
            <nuxt-link to="/">Tencnología</nuxt-link>
            <nuxt-link to="/">Equipo</nuxt-link>
            <nuxt-link to="/">Contacto</nuxt-link>
          </nav>
        </section>
        <section class="simple-footer__legal">
          <p>© LKMX Software Development 2021.</p>
        </section>
      </simple-footer>
    </template>
  </simple-layout>
</template>

<script>
export default {
  data: function() {
    return {
      isNavOpen: false
    };
  },
  methods: {
    toggleMenu() {
      const anime = this.$anime;

      this.isNavOpen = !this.isNavOpen;

      const navEl = document.querySelector("nav.main-navigation");
      const navAnchorEl = document.querySelectorAll("nav.main-navigation a");
      const bodyEl = document.querySelector("body");
      const navIsClosed = () => {
        navEl.classList.remove("navIsOpen");
        bodyEl.classList.remove("fixed");
        this.isNavOpen = false;
      };
      const imgMenuOpenEl = document.querySelector(".menu-open");
      const imgMenuCloseEl = document.querySelector(".menu-close");

      const menuIconAnimeExit = anime.timeline({
        easing: "easeInElastic(1, .6)",
        duration: 500
      });

      const menuIconAnimeEntry = anime.timeline({
        easing: "easeOutElastic(1, .6)",
        duration: 500
      });

      if (this.isNavOpen) {
        navEl.classList.add("navIsOpen");
        bodyEl.classList.add("fixed");

        for (const anchor of navAnchorEl) {
          anchor.addEventListener("click", navIsClosed);
        }
        menuIconAnimeExit
        .add({
          targets: imgMenuOpenEl,
          rotate: -45,
          opacity: 0,
        })
        .add({
          targets: imgMenuCloseEl,
          rotate: 0,
          opacity: 1,
        });
      } else {
        navIsClosed();
        menuIconAnimeEntry
        .add({
          targets: imgMenuCloseEl,
          rotate: 45,
          opacity: 0,
        })
        .add({
          targets: imgMenuOpenEl,
          rotate: 0,
          opacity: 1,
        })
      }
    }
  },
  mounted() {}
};
</script>

<style lang="scss">
@import "@lkmx/flare/src/functions/_respond-to.scss";
@import "~/assets/sass/_space";
@import "~/assets/sass/_general-functions";

#__nuxt {
  .simple-header {
    z-index: 10;
    .--flare {
      .content {
        .box {
          @include respond-to("<=m") {
            @include grid($gaf: column, $ji: start, $gtc: 100px 1fr);
          }
          .header__menu {
            display: none;
            @include respond-to("<=m") {
              // @include grid();
              display: block;
              padding: var(--f-gutter-s);
              height: var(--f-gutter-xl);
              width: var(--f-gutter-xl);
              img {
                position: absolute;
                &.menu-close {
                  opacity: 0;
                  transform: rotate(45);
                }
              }
            }
          }
          .logo-container {
            @include grid();
            height: 88px;
            .header__logo {
              height: 88px;
              padding: var(--f-gutter-s) var(--f-gutter);
              @include respond-to("<=m") {
                width: 88px;
                position: absolute;
                left: 0;
                right: 0;
                top: var(--f-gutter);
                margin-left: auto;
                margin-right: auto;
              }
            }
          }
          .main-navigation {
            @include respond-to("<=m") {
              display: none;
              &.navIsOpen {
                @include grid();
                grid-template-rows: repeat(5, minmax(10px, 60px));
                grid-auto-flow: row;
                // display: block;
                z-index: -1;
                background: var(--color-neutral-black);
                position: fixed;
                overflow-y: scroll;
                top: var(--f-header-height);
                right: 0;
                bottom: 0;
                left: 0;
                height: calc(100vh - var(--f-header-height));
                width: 100%;
                &::-webkit-scrollbar {
                  width: 0; /* Remove scrollbar space */
                  background: transparent; /* Optional: just make scrollbar invisible */
                }
              }
            }
            a {
              font-family: var(--f-mono-text-font);
              text-decoration: none;
            }
          }
        }
      }
    }
  }

  .simple-footer {
    @include spacer();
    .--flare-frame {
      mix-blend-mode: unset !important;
      z-index: 1 !important;
    }
    // background-color: white;
    .content {
      .box {
        @include grid($g: var(--f-gutter-xxl), $ji: start);
      }
    }
    // min-height: 600px;
    background-color: var(--color-accent-cyan);
    * {
      color: var(--color-neutral-black);
    }
    &__content {
      @include grid($g: var(--f-gutter), $ai: start, $gtc: 1fr 1fr);
      @include spacer(m);
      grid-auto-flow: column;
      @include respond-to("<=m") {
        @include grid($g: var(--f-gutter-xxl));
        grid-auto-flow: row;
      }

      &__brand {
        @include grid($g: var(--f-gutter), $ji: start);
        img {
          width: 144px;
        }
        p {
          font-family: var(--f-mono-text-font);
        }
        .social {
          @include grid();
          grid-auto-flow: column;
          img {
            width: 24px;
            height: 24px;
          }
        }
      }
      &__navigation {
        @include grid($ji: end, $g: var(--f-gutter));
        @include respond-to("<=m") {
          @include grid($ji: start);
        }
        width: 100%;
        * {
          margin: 0;
          font-family: var(--f-mono-text-font);
        }
        h6 {
          font-size: 20px;
          margin-bottom: var(--f-gutter);
        }
        a {
          color: var(--color-accent-blue-02);
          text-decoration: none;
        }
      }
    }
    &__legal {
      p {
        font-family: var(--f-mono-text-font);
      }
    }
  }
}
</style>
