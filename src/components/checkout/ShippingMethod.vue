<template>
  <div id="shipping-detail" class="align-left">
    <br>
    <h4>Cuando quisieras recibir el producto?</h4>
    <hr>
    <div v-if="isSessionActive">
      <b-form-group>
        <label for="method">Plazo</label>
        <b-form-select
          v-model="shippingMethod"
          size="sm"
          name="method"
          id="method"
          @input="selected()"
        >
          <option :value="null" disabled>Porfavor seleccione una opcion</option>
          <option v-for="(ship,sid) in shippingMethods" v-bind:key="sid" :value="ship">{{ship.name}}</option>
        </b-form-select>
      </b-form-group>
      <p class="info">Porfavor seleccione el tiempo de entrega para poder continuar</p>
    </div>
    <div v-else>
      <div class="empty-info">
        <p>Nada que mostrar</p>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import notification from '@/services/notificationService';

export default {
  name: 'ShippingMethod',
  data() {
    return {
      shippingMethods: [
        {
          _id: '1 a 3 dias',
          name: '1 a 3 dias',
        },
        {
          _id: '4 a 6 dias',
          name: '4 a 6 dias',
        },
        {
          _id: '7 a 9 dias',
          name: '7 a 9 dias',
        },
        {
          _id: 'Otro, escribenos por Whatsapp',
          name: 'Otro, escribenos por Whatsapp',
        },        
      ],
    };
  },
  methods: {
    async selected() {
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
  },
  computed: {
    shippingMethod: {
      get() {
        return this.$store.getters['shippingStore/shippingMethod'];
      },

      set(val) {
        this.$store.commit('shippingStore/setShippingMethod', val);
      },
    },

    ...mapGetters({
      isSessionActive: 'authStore/isSessionActive',
      selectedAddress: 'shippingStore/getSelectedAddress',
      checkoutInitiated: 'cartStore/checkoutInitiated',
    }),
  },
};
</script>

<style lang="scss">
#shipping-detail {
  margin-top: 1rem;
}
</style>
