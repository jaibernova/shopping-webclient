<template>
  <div id="app">
    <notifications
      group="all"
      classes="vue-notification main-notification"
      width="100%"
      position="bottom center"
    />
    <notifications
      group="toast"
      class="toast-noti"
      classes="vue-notification toast-notification"
      position="top right"
    />
    <div v-if="isLoading">
      <fingerprint-spinner class="spinner" :animation-duration="1500" :size="150" color="#136a8a"/>
    </div>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
    <a href="https://api.whatsapp.com/send?phone=+573174081631&text=Hola%21%20Quisiera%20m%C3%A1s%20informaci%C3%B3n%20sobre%20este%20producto" class="float" target="_blank">
      <i class="fa fa-whatsapp my-float"></i>
    </a>
    <router-view/>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import { FingerprintSpinner } from 'epic-spinners';
import { eventHub } from '@/utils/EventHub';
import moment from 'moment';

export default {
  name: 'app',
  components: {
    FingerprintSpinner,
  },

  async created() {
    eventHub.$on('before-request', this.setLoading);
    eventHub.$on('request-error', this.unsetLoading);
    eventHub.$on('after-response', this.unsetLoading);
    eventHub.$on('response-error', this.unsetLoading);

    await this.$store.dispatch('authStore/initiateAppSession');
    if (this.isSessionActive) {
      this.initiateApp();
    } else {
      this.$store.commit('shippingStore/resetAddresses');
      this.$store.commit('cartStore/resetOrders');
    }
  },

  data() {
    return {
      refCount: 0,
      isLoading: false,
    };
  },

  beforeDestroy() {
    eventHub.$off('before-request', this.setLoading);
    eventHub.$off('request-error', this.unsetLoading);
    eventHub.$off('after-response', this.unsetLoading);
    eventHub.$off('response-error', this.unsetLoading);
  },

  methods: {
    async initiateApp() {
      try {
        await this.$store.dispatch('cartStore/getCart');
        await this.$store.dispatch('shippingStore/addressAction', {
          address: null,
          action: 'get',
        });

        if (this.checkoutInitiated) {
          const reqObj = {
            address: this.selectedAddress,
            shippingMethod: this.shippingMethod,
          };

          await this.$store.dispatch('cartStore/createCheckout', reqObj);
        }
      } catch (error) {
        console.log(error);
      }
    },
    setLoading() {
      this.refCount += 1;
      this.isLoading = true;
    },

    checkSessionTimeout() {
      const dt = localStorage.getItem('sessionDT');
      if (!dt) {
        return false;
      }
      const diff = moment.duration(moment().diff(moment(dt)));
      if (diff.asMinutes() >= 30) return false;

      localStorage.setItem('sessionDT', moment().format());
      return true;
    },

    unsetLoading() {
      if (this.isSessionActive) {
        const isActive = this.checkSessionTimeout();

        if (!isActive) {
          console.log('This is also happening while unsetting loading');
          this.$store.commit('shippingStore/resetAddresses');
          this.$store.commit('cartStore/resetOrders');
          this.$store.commit('authStore/logoutUser');
        }
      }

      if (this.refCount > 0) {
        this.refCount -= 1;
        this.isLoading = this.refCount > 0;
      }
    },
  },

  computed: {
    ...mapGetters({
      isSessionActive: 'authStore/isSessionActive',
      shippingMethod: 'shippingStore/shippingMethod',
      selectedAddress: 'shippingStore/getSelectedAddress',
      checkoutInitiated: 'cartStore/checkoutInitiated',
    }),
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Karla');

#app {
  font-family: 'Quicksand','Raleway', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
  color: rgba(34,34,34,.7);
  /* height: 100%; */
  font-size: 0.93em;
}

@media (max-width: 768px) {
  #app {
    overflow: hidden;
  }
}

html,
body {
  margin: 0px;
  height: 100%;
  zoom: 101%;
}

.container {
  margin: 0px;
}

.toast-notification {
  /* margin-top: 100px !important; */
  font-size: 0.8em !important;
  padding: 20px 10px !important;
}

.toast-noti {
  top: 100px !important;
}

.spinner {
  position: fixed !important;
  top: 0px !important;
  height: 100vh !important;
  width: 100% !important;
  z-index: 10000 !important;
  background: rgba(255, 255, 255, 0.8) !important;
}

.float{
	position:fixed;
	width:60px;
	height:60px;
	bottom:40px;
	right:40px;
	background-color:#25d366;
	color:#FFF;
	border-radius:50px;
	text-align:center;
  font-size:30px;
	box-shadow: 2px 2px 3px #999;
  z-index:100;
}
.float:hover {
	text-decoration: none;
	color: #25d366;
  background-color:#fff;
}

.my-float{
	margin-top:16px;
}
</style>
