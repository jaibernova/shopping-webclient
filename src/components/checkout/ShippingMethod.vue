<template>
  <div id="shipping-detail" class="align-left">
    <br>
    <h4>Metodo de envio</h4>
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
          <option :value="null" disabled>Please select an option</option>
          <option v-for="(ship,sid) in shippingMethods" v-bind:key="sid" :value="ship">{{ship.name}}</option>
        </b-form-select>
      </b-form-group>
      <p class="info">Please select the shipping method to get final prices</p>
    </div>
    <div v-else>
      <div class="empty-info">
        <p>Nothing to display</p>
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
          _id: 'Hoy mismo',
          name: 'Hoy mismo',
        },
        {
          _id: '1 a 3 dias',
          name: '1 a 3 dias',
        },
        {
          _id: '3 a 5 dias',
          name: '3 a 5 dias',
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
