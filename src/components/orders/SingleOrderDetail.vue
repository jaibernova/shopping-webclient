<template>
  <div id="single-order-detail" class="align-left">
    <div class="space"></div>
    <div v-if="order">
      <h4>
        <router-link to="/orders"><font-awesome-icon icon="chevron-left"/></router-link>
        El detalle de tu orden
      </h4>

      <b-card-group deck class="mb-3">
        <b-card bg-variant="light">
          <template v-slot:header>
            <div class="info">
              <!-- Status goes here -->
              Estado:
              <span
                v-bind:class="{'green': order.overall_status != 'CANCELLED',
                'red': order.overall_status === 'CANCELLED'}"
              >
                <strong>{{order.overall_status}}</strong>
              </span><br>

              <strong>Orden #</strong>
              {{order._id}}<br>
              <strong>Fecha de creacion</strong>
              {{dateFormat(order.auditLog.createdOn)}}
            </div>
          </template>
          <div class="card-text card-font">
            <p>Peso total: <strong>{{order.cart.totalWeight.quantity}} {{order.cart.totalWeight.unit}}</strong></p>
            <p>Sub Total: <strong>$ {{order.cart.subTotalPrice.amount.toLocaleString("de-DE")}}</strong></p>
            <p>Cargo adicional: <strong>$ {{order.cart.serviceCharge.amount.toLocaleString("de-DE")}}</strong></p>
            <p>Costo de envio: <strong>$ {{order.cart.shippingPrice.amount.toLocaleString("de-DE")}}</strong></p>
            <p>Tarifa: <strong>$ {{order.cart.tariffPrice.amount.toLocaleString("de-DE")}}</strong></p>
            <p>Precio total: <strong>$ {{order.cart.totalPrice.amount.toLocaleString("de-DE")}}</strong></p>
          </div>
        </b-card>

        <b-card bg-variant="light" header="Datos de envio" v-if="addr">
          <div class="card-text card-font">
            <p>
              {{addr.firstName}} {{addr.lastName}} </p>
            <p>
            {{addr.addressLine1}}</p>

            <p v-if="addr.addressLine2">
              {{addr.addressLine2}}
            </p>
            <p>
            {{addr.city}}, {{addr.state}}</p>
            <p>
            {{addr.country}} {{addr.zipCode}}</p>
            <p>
            {{addr.mobilePhone}}</p>

          </div>
        </b-card>

        <b-card bg-variant="light" header="Metodo de pago" v-if="payments.length > 0">
          <div class="card-text card-font">
            <p v-for="(payment, pind) in payments" v-bind:key="pind">
              <strong>
                <!-- {{payment.source}}&nbsp;&nbsp;&nbsp;&nbsp; -->
                <!-- <span class="green info">{{payment.type}}</span>                 -->
                <span class="green info">ELECTRONICO</span>
              </strong>
              <br>
              <!-- $ {{payment.amount_in_payment_currency.amount }}  -->
            </p>
          </div>
        </b-card>
      </b-card-group>

      <div class="cart-items">
        <h5>Productos comprados</h5>
        <br>
        <div v-for="(item, iind) in order.cart.items" v-bind:key="iind">
          <single-order-item :item="item"/>
          <hr v-if="iind != order.cart.items.length -1">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProxyUrls from '@/constants/ProxyUrls';
import notification from '@/services/notificationService';
import moment from 'moment';
import SingleOrderItem from '@/components/orders/SingleOrderItem.vue';

export default {
  name: 'SingleOrderDetail',
  props: {
    id: {
      required: true,
      type: String,
    },
  },
  components: {
    SingleOrderItem,
  },

  data() {
    return {
      order: null,
    };
  },

  filters: {
    formattedDate(date) {
      if (date) {
        return moment(date).format('MMMM Do, YYYY');
      }
      return '';
    },

    formattedAmount(amt) {
      const val = parseFloat(amt);

      return val.toFixed(2);
    },
  },


  async created() {
    this.order = null;

    try {
      const { data } = await this.$axios({
        url: ProxyUrls.orderDetail + this.id,
        method: 'get',
      });

      if (data && data.httpStatus === 200) {
        this.order = data.responseData;
      } else {
        throw new Error();
      }
    } catch (error) {
      notification.error(this, 'Could not open the details page at the moment', 'all');
      this.$router.push('/orders');
    }
  },

  methods: {
    dateFormat(date) {
      if (date) {
        return moment(date).format('MMMM Do, YYYY');
      }
      return '';
    },
  },

  computed: {
    addr() {
      return this.order ? this.order.mailing_address : null;
    },

    payments() {
      return this.order && this.order.payment_info ? this.order.payment_info : null;
    },
  },
};
</script>

<style lang="scss" scoped>
#single-order-detail {
  min-height: 90vh;
  width: 70%;
  margin: auto;
  margin-top: 1em;

  h4 {
    margin-bottom: 2rem;
  }

  .card-font {
    font-size: small;
  }

  .card-text{
    p{
    margin-bottom: 0.5rem;

    }
  }

  .cart-items {
    margin-top: 2rem;
  }
}
</style>
