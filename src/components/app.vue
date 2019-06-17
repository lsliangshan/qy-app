<template>
  <f7-app :params="f7params">
    <!-- Status bar overlay for fullscreen mode-->
    <f7-statusbar></f7-statusbar>

    <!-- Left panel with cover effect when hidden -->
    <f7-panel left
              reveal
              theme-dark>
      <f7-view>
        <f7-page>
          <f7-navbar title="Left Panel"></f7-navbar>
          <f7-block-title>Left View Navigation</f7-block-title>
          <f7-list>
            <f7-list-item link="/left-page-1/"
                          title="Left Page 1"></f7-list-item>
            <f7-list-item link="/left-page-2/"
                          title="Left Page 2"></f7-list-item>
          </f7-list>
          <f7-block-title>Control Main View</f7-block-title>
          <f7-list>
            <f7-list-item link="/about/"
                          view=".view-main"
                          panel-close
                          title="About"></f7-list-item>
            <f7-list-item link="/form/"
                          view=".view-main"
                          panel-close
                          title="Form"></f7-list-item>
            <f7-list-item link="#"
                          view=".view-main"
                          back
                          panel-close
                          title="Back in history"></f7-list-item>
          </f7-list>
        </f7-page>
      </f7-view>
    </f7-panel>

    <!-- Right panel with reveal effect-->
    <f7-panel right
              reveal
              theme-dark>
      <f7-view>
        <f7-page>
          <f7-navbar title="Right Panel"></f7-navbar>
          <f7-block>Right panel content goes here</f7-block>
        </f7-page>
      </f7-view>
    </f7-panel>

    <!-- Your main view, should have "view-main" class -->
    <f7-view main
             class="safe-areas"
             url="/"></f7-view>

    <!-- Popup -->
    <f7-popup id="my-popup">
      <f7-view>
        <f7-page>
          <f7-navbar title="Popup">
            <f7-nav-right>
              <f7-link popup-close>Close</f7-link>
            </f7-nav-right>
          </f7-navbar>
          <f7-block>
            <p>Popup content goes here.</p>
          </f7-block>
        </f7-page>
      </f7-view>
    </f7-popup>

    <f7-login-screen id="my-login-screen">
      <f7-view>
        <f7-page login-screen>
          <f7-login-screen-title>Login</f7-login-screen-title>
          <f7-list form>
            <f7-list-input type="text"
                           name="username"
                           placeholder="Your username"
                           :value="username"
                           @input="username = $event.target.value"></f7-list-input>
            <f7-list-input type="password"
                           name="password"
                           placeholder="Your password"
                           :value="password"
                           @input="password = $event.target.value"></f7-list-input>
          </f7-list>
          <f7-list>
            <f7-list-button title="Sign In"
                            login-screen-close
                            @click="alertLoginData"></f7-list-button>
            <f7-block-footer>
              Some text about login information.<br>Click "Sign In" to close Login Screen
            </f7-block-footer>
          </f7-list>
        </f7-page>
      </f7-view>
    </f7-login-screen>
  </f7-app>
</template>
<script>
import cordovaApp from '../js/cordova-app.js';
import routes from '../js/routes.js';

export default {
  data () {
    const welcomescreen_slides = [
      {
        id: 'slide0',
        title: 'Slide 1',
        picture: '<div class="full_tutorialicon"><img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1560800490636&di=5328a09ed7dfa9012405e0ed5e8140d1&imgtype=0&src=http%3A%2F%2Fhbimg.b0.upaiyun.com%2Fdd0f2a1486e73d6fc7d10ce91816c8e43730714a50dd0-qgN5GZ_fw658"/></div>',
        text: 'Welcome to this tutorial. In the <a class="tutorial-next-slide" href="#">next steps</a> we will guide you through a manual that will teach you how to use this app.<br><br>Swipe to continue →'
      },
      {
        id: 'slide1',
        title: 'Slide 2',
        picture: '<div class="full_tutorialicon"><img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1560800490637&di=c8e73ba3c0889647425bfac877f2c2d5&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201510%2F25%2F20151025233655_nrPax.thumb.700_0.jpeg"/></div>',
        text: 'This is slide 2<br><br>Swipe to continue →'
      },
      {
        id: 'slide2',
        title: 'Slide 3',
        picture: '<div class="full_tutorialicon"><img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1560800490633&di=26121de57f51b54a65542e0e09a4a32b&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201702%2F05%2F20170205213047_r5nvC.jpeg"/></div>',
        text: 'This is slide 3<br><br>Swipe to continue →'
      },
      {
        id: 'slide3',
        // title: 'NO TITLE', 
        picture: '<div class="full_tutorialicon"><img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1560800490632&di=9ac858dc03a3a7a272c05757aa87f960&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fblog%2F201512%2F01%2F20151201134900_FsA5S.jpeg"/></div>',
        text: 'Thanks for reading! Enjoy this app or go to <a class="tutorial-previous-slide" href="#">previous slide</a>.<br><br><a class="tutorial-close-btn" href="#">开始</a>'
      }
    ]
    const options = {
      // 'bgcolor': '#0A510F',
      'fontcolor': '#fff',
      closeButtonText: '跳过',
      loop: true,
      parallax: false,
      parallaxSpeed: 600,       // parallax default speed
      parallaxBackgroundImage: '', // parallax default background image
      parallaxBackground: '-23%', // parallax default background effect
      parallaxSlideElements: {
        title: -100,
        subtitle: -300,
        text: -600
      }
    }
    return {
      // Framework7 Parameters
      f7params: {
        id: 'com.dei2.qy-app', // App bundle ID
        name: 'qy-app', // App name
        theme: 'auto', // Automatic theme detection
        name: 'welcomescreen-demo',
        welcomescreen: { // Setup welcomescreen plugin
          slides: welcomescreen_slides,
          options: options
        },
        // App root data
        data: function () {
          return {
            user: {
              firstName: 'John',
              lastName: 'Doe',
            },

          };
        },

        // App routes
        routes: routes,
        // Enable panel left visibility breakpoint
        panel: {
          leftBreakpoint: 960,
        },


        // Input settings
        input: {
          scrollIntoViewOnFocus: this.$device.cordova && !this.$device.electron,
          scrollIntoViewCentered: this.$device.cordova && !this.$device.electron,
        },
        // Cordova Statusbar settings
        statusbar: {
          overlay: this.$device.cordova && this.$device.ios || 'auto',
          iosOverlaysWebView: true,
          androidOverlaysWebView: false,
        },
      },

      // Login screen data
      username: '',
      password: '',
    }
  },
  methods: {
    alertLoginData () {
      this.$f7.dialog.alert('Username: ' + this.username + '<br>Password: ' + this.password);
    }
  },
  mounted () {
    this.$f7ready((f7) => {
      // f7.welcomescreen.open()
      // Init cordova APIs (see cordova-app.js)
      if (f7.device.cordova) {
        cordovaApp.init(f7);
      }
      // Call F7 APIs here
      f7.$(document).on('click', '.welcomescreen-closebtn', function () {
        f7.welcomescreen.close()
      })
      f7.$(document).on('click', '.tutorial-close-btn', function () {
        f7.welcomescreen.close()
      })
      f7.$(document).on('click', '.tutorial-previous-slide', function () {
        f7.welcomescreen.previous()
      })
      f7.$(document).on('click', '.tutorial-next-slide', function () {
        f7.welcomescreen.next()
      })
    });
  }
}
</script>
<style>
.tutorialicon {
  font-size: 250px;
  line-height: 170px;
}

.tutorial-close-btn {
  background: #fff;
  padding: 10px;
  border-radius: 10px;
  color: #0da6ec;
}

.welcomescreen-container .welcomescreen-text a {
  color: #fff;
  text-decoration: underline;
}

.tutorial-close-btn {
  background: #fff;
  padding: 10px;
  border-radius: 10px;
  color: #0da6ec !important;
  text-decoration: none !important;
}

.welcomescreen-picture {
  position: absolute !important;
  left: 0px !important;
  top: 0px !important;
  z-index: 0 !important;
  margin: 0px !important;
  width: 100% !important;
  height: 100% !important;
}
.welcomescreen-picture .full_tutorialicon {
  margin: 0px !important;
  width: 100% !important;
  height: 100% !important;
  overflow: hidden !important;
}
.welcomescreen-picture .full_tutorialicon img {
  max-width: 100%;
  max-height: 100%;
}
.welcomescreen-title {
  position: absolute !important;
  z-index: 1 !important;
}
.page-content {
  padding: 20px;
}
</style>
