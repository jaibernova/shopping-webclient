<template>
  <div class="checkout">
    <div class="space"></div>
    <h2 class="featured-title">Checkout</h2>

    <b-row>
      <b-col md="6">
        <b-card bg-variant="light" title="Direccion de envio" class="text-left">
          <shipping-detail @selected="addressSelected"/>
          <shipping-method/>
          <transition
            name="shipping-form-anim"
            enter-active-class="animated slideInLeft faster"
            leave-active-class="animated slideOutLeft faster"
          >
            <payment-detail v-if="checkoutInitiated"/>

          </transition>
        </b-card>
      </b-col>
      <b-col md="6">
        <b-card bg-variant="light" title="Tu carrito" class="text-left">
          <order-detail/>
        </b-card>
      </b-col>
    </b-row>

    <div class="bottom-space"></div>
    <div class="checkout-button">
      <div v-if="isSessionActive">
        <div
          v-if="emailConfirmed && !checkoutInitiated && carts.length > 0"
        >
          <b-button
            size="lg"
            class="full-width primary-button"
            @click="handleCheckout()"
          >Obtener el precio final</b-button>
          <br />
          <br />
          
        </div>
        
        <div v-else-if="!emailConfirmed">
        <!-- <div class="bottom-space"></div>
        <div class="bottom-space"></div>
        <div class="bottom-space"></div>
        <div class="bottom-space"></div>         -->
          <p>Por favor confirma tu correo antes de realizar el pago.
            Presiona el boton inferior para reenviar el correo de confirmacion.</p>
          <b-button
            class="secondary-button"
            @click="resendEmailConfirmation()"
          >Reenviar correo de confirmacion</b-button>   
          <p>Si ya confirmaste tu correo exitosamente.
            Porfavor presiona el boton inferior para volver a iniciar sesion y asi refrescar la sesion actual.</p>
          <b-button
            class="secondary-button"
            @click="iniciarSesionNuevamente()"
          >Ir a iniciar sesion </b-button>              
        </div>


        <div v-if="checkoutInitiated">

        </div>
      </div>
      <div v-else>
        <p>Porfavor inicia sesion antes de iniciar el proceso de pago</p>
      </div>
    </div>
  </div>
</template>

<script>
import ShippingDetail from '@/components/checkout/ShippingDetail.vue';
import OrderDetail from '@/components/checkout/OrderDetail.vue';
import PaymentDetail from '@/components/checkout/PaymentDetail.vue';
import ProxyUrls from '@/constants/ProxyUrls';
import ShippingMethod from '@/components/checkout/ShippingMethod.vue';
import { mapGetters } from 'vuex';
import notification from '@/services/notificationService';

export default {
  name: 'Checkout',
  components: {
    ShippingDetail,
    OrderDetail,
    PaymentDetail,
    ShippingMethod,
  },

  data() {
    return {
      // selectedAddress: {},
      payment: {},
    };
  },

  created() {
    // if(!this.isSessionActive || this.carts.length <= 0){
    //   this.$router.push('/');
    // }
  },

  methods: {

    async addressSelected(selected) {
      this.$store.commit('shippingStore/addressSelected', selected);
      if (!this.checkoutInitiated) return;
      try {
        await this.$store.dispatch('cartStore/createCheckout', {
          address: this.selectedAddress,
          shippingMethod: this.shippingMethod,
        });
      } catch (error) {
        notification.error(
          this,
          'Something went haywire while trying to recalculate the prices. Please try again by changing address.',
        );
      }
    },

    async handleCheckout() {
      if (!this.selectedAddress || !this.shippingMethod) {
        notification.warn(this, 'Por favor selecciona tiempo de entrega y direccion de envio.');
        return;
      }
      await this.$store.dispatch('cartStore/createCheckout', {
        address: this.selectedAddress,
        shippingMethod: this.shippingMethod,
      });
    },

    async resendEmailConfirmation() {
      try {
        const { data } = await this.$axios({
          method: 'get',
          url: ProxyUrls.resendEmailConfirmation + this.$store.getters['authStore/getEmail'],
        });

        if (data && data.httpStatus === 200) {
          this.$notify({
            group: 'all',
            type: 'success',
            text: 'El correo de confirmacion fue enviado satisfactoriamente. Porfavor revisa tu correo.',
          });
        }
      } catch (err) {
        this.$notify({
          group: 'all',
          type: 'success',
          text: 'El correo fue confirmado exitosamente. Ingresa nuevamente porfavor',
        });
        this.$store.commit('cartStore/resetOrders');
        this.$store.commit('shippingStore/resetAddresses');
        this.$router.push('/login?previousPath=%2F');        
      }
    },
    async iniciarSesionNuevamente() {
        this.$store.commit('cartStore/resetOrders');
        this.$store.commit('shippingStore/resetAddresses');
        this.$router.push('/login?previousPath=%2F');     
    },    
  },
  

  computed: {
    ...mapGetters({
      selectedAddress: 'shippingStore/getSelectedAddress',
      carts: 'cartStore/getCart',
      isSessionActive: 'authStore/isSessionActive',
      checkoutInitiated: 'cartStore/checkoutInitiated',
      emailConfirmed: 'authStore/emailConfirmed',
    }),

    shippingMethod: {
      get() {
        return this.$store.getters['shippingStore/shippingMethod'];
      },
      set(val) {
        this.$store.commit('shippingStore/setShippingMethod', val);
      },
    },
  },
};
</script>

<style lang="scss">


.checkout {
  padding: 0rem 2rem;
  margin-bottom: 10px;
  padding-bottom: 3rem;
  min-height: 60vh;
  position: relative;
  background-color: #eaecee;

  .bottom-space {
    height: 10px;
  }

  .checkout-button {
    margin-top: 1px;
    padding: 0rem 2rem;
    position: relative;
    bottom: 0;
    left: 0;
    right: 0;
  }
}
</style>
