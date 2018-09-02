<template>
  <md-toolbar class="main-header" :class="mainHeaderClasses" :md-elevation="mainHeaderElevation">
    <div class="md-toolbar-row">
      <div class="md-toolbar-section-start">
        <md-button to="/" class="md-icon-button logo">
          <logo-vue-material :animated="isHome" />
        </md-button>

      </div>

      <div class="md-toolbar-section-end">

         
            <md-tabs class="md-accent" md-alignment="right">
              <md-tab id="tab-home" md-label="Home"></md-tab>
              <md-tab id="tab-pages" md-label="Pages"></md-tab>
              <md-tab id="tab-posts" md-label="Posts"></md-tab>
              <md-tab id="tab-favorites" md-label="Favorites"></md-tab>
            </md-tabs>





        <md-button href="http://github.com/vuematerial/vue-material" target="_blank" class="md-icon-button">
          <md-icon md-src="/assets/icon-github.svg" class="icon-github" />
        </md-button>
      </div>
    </div>
  </md-toolbar>
</template>

<script>
  // import TabAlignment from '/docs/app/pages/Components/Tabs/examples/TabAlignments'
  import { mapState, mapActions, mapMutations } from 'vuex'
  import * as types from 'store/mutation-types'

  export default {
    name: 'MainHeader',
    data: () => ({
      themeMenuActive: false
    }),
    computed: {
      ...mapState({
        stateTitle: 'pageTitle',
        isSplash: 'splashPage',
        currentTheme: 'theme'
      }),
      pageTitle () {
        const { stateTitle } = this

        return stateTitle && stateTitle
      },
      isHome () {
        return this.$route.path === '/'
      },
      mainHeaderClasses () {
        const { isSplash } = this

        return {
          'md-transparent': isSplash,
          'md-elevation-2': !isSplash,
          'md-primary': !this.currentTheme.includes('default')
        }
      },
      mainHeaderElevation () {
        if (this.isSplash) {
          return 0
        }

        return 2
      }
    },
    methods: {
      ...mapActions({
        setCurrentTheme: types.CHANGE_THEME
      }),
      ...mapMutations({
        showMenu: types.SHOW_MENU
      }),
      setTheme (theme) {
        this.setCurrentTheme(theme)

        window.setTimeout(() => {
          this.themeMenuActive = false
        }, 50)
      },
      getPrimaryClass (theme) {
        if (theme === this.currentTheme) {
          return 'md-primary'
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "~vue-material/components/MdAnimation/variables";
  @import "~vue-material/components/MdElevation/mixins";
  @import "~components/MdLayout/mixins";

  .main-header {
    margin: auto;
    max-width: 100%;
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    z-index: 3;
    transition: .3s $md-transition-default-timing,
                box-shadow .4s .1s $md-transition-stand-timing;
    will-change: box-shadow, max-width, background-color;

    &.md-transparent {
      background-color: #303030;
    }
  }

  div.md-toolbar-row {
    max-width: 100%;
    transition: max-width .3s $md-transition-default-timing;
    will-change: max-width;

    .md-title {
      flex: 1;

      .md-title {
        margin-left: 0;
      }
    }

    h1 {
      display: inline-block;
    }
  }

  .logo {
    &:not(.md-theme-default):not(.md-theme-dark):not(.md-theme-default-dark) {
      background-color: rgba(#fff, .2);
    }

    @include md-layout-xsmall {
      display: none;
    }

    >>> .md-ripple {
      padding: 0;
      overflow: visible;
    }

    >>> .md-button-content {
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .logo-vue-material {
      max-width: 72%;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
    }
  }

  .menu {
    display: none;

    @include md-layout-xsmall {
      display: inline-block;
    }
  }

  .fixed-theme-selector {
    display: none;

    @include md-layout-medium {
      display: block;
    }
  }

  .fixed-theme-selector-items {
    min-width: 118px;
  }

  .floating-theme-selector {
    @include md-elevation(2);
    max-height: 40px;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    overflow: hidden;
    position: fixed;
    top: 50%;
    right: 0;
    transform: translate3D(124px, -50%, 0);
    border-radius: 20px 0 0 20px;
    transition: .3s $md-transition-default-timing;
    transition-property: border-radius, max-height, transform;
    will-change: border-radius, max-height, transform;

    &.active {
      max-height: 215px;
      border-radius: 2px 0 0 2px;
      transform: translate3d(0, -50%, 0);

      >>> .md-list {
        padding: 4px 0;
        opacity: 1;

        .md-list-item:first-child {
          ~ .md-list-item {
            opacity: 1;
          }

          .md-list-item-content {
            padding-left: 16px;
            font-weight: 700;
            border-bottom: 1px solid rgba(#000, .12);
          }
        }
      }
    }

    @include md-layout-medium {
      display: none;
    }
  }

  .floating-theme-selector-items {
    >>> .md-list {
      padding: 0;
      border-bottom-left-radius: 2px;

      .md-list-item:first-child {
        ~ .md-list-item {
          opacity: 0;
          transition: opacity .35s $md-transition-drop-timing;
          will-change: opacity;
        }

        .md-list-item-content {
          padding-left: 8px;
        }
      }

      .md-list-item-content {
        justify-content: flex-start;
      }
    }
  }
</style>
