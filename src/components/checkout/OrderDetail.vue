<template>
  <div class="order-detail align-left">
      <ul class="orders">
        <li v-for="(item, itemIndex) in orders" v-bind:key="itemIndex">
          <b-row>
            <b-col md="3">
              <div
                class="order-img order-desc"
                v-if="item.product.thumbnailUrls && item.product.thumbnailUrls.length > 0"
                @click="gotoProduct(item.product)"
                :style="orderPicture(item.product.thumbnailUrls[0])"
              ></div>
            </b-col>
            <b-col md="5">
              <div class="order-desc" @click="gotoProduct(item.product)">
                {{item.product.name}}
                <br>
                <div style="font-size: 12px">
                  <span
                    v-for="(custom, cid) of item.customizations"
                    v-bind:key="cid"
                  >{{custom | customDisplay}} |</span>
                </div>
              </div>
              <span class="delete" @click="deleteSelected(item)">Borrar</span>
            </b-col>
            <b-col class="align-right">
              <b-form-select
                size="sm"
                v-model="item.counts"
                :options="countOptions"
                @change.native="updateCartItem(item)"
                class="mb-3"
              />
            </b-col>
            <b-col
              class="align-right"
            >$ {{item ? item.aggregatedPrice.amount.toLocaleString("de-DE") : ''}}</b-col>
          </b-row>
        </li>
      </ul>
    <hr>

    <div class="total-line" v-if="orders && orders.length > 0">
      <b-row>
        <!-- <hr> -->
        <b-col cols="6" class="align-right">
          <strong>Peso total</strong>
        </b-col>
        <b-col
          class="align-right"
        >{{parseFloat(totalWeight.quantity).toFixed(2)}} {{totalWeight.unit}}</b-col>
      </b-row>

      <b-row>
        <!-- <hr> -->
        <b-col cols="6" class="align-right">
          <strong>Sub Total</strong>
        </b-col>
        <b-col class="align-right">$ {{(subtotal.amount.toLocaleString("de-DE"))}}</b-col>
      </b-row>

      <b-row v-if="shippingPrice">
        <!-- <hr> -->
        <b-col cols="6" class="align-right">
          <strong>Costo de envio</strong>
        </b-col>
        <b-col
          class="align-right"
        >$ {{(shippingPrice.amount.toLocaleString("de-DE"))}}</b-col>
      </b-row>

      <b-row v-if="serviceCharge">
        <!-- <hr> -->
        <b-col cols="6" class="align-right">
          <strong>Cargos por servicio</strong>
        </b-col>
        <b-col
          class="align-right"
        >$ {{(serviceCharge.amount)}}</b-col>
      </b-row>



      <b-row v-if="tariffPrice">
        <!-- <hr> -->
        <b-col cols="6" class="align-right">
          <strong>Tarifa</strong>
        </b-col>
        <b-col
          class="align-right"
        >$ {{(tariffPrice.amount.toLocaleString("de-DE"))}}</b-col>
      </b-row>

      <br>
      <b-row v-if="cartTotal">
        <!-- <hr> -->
        <b-col cols="6" class="align-right">
          <strong>Total</strong>
        </b-col>
        <b-col class="align-right">
          <strong>$ {{(cartTotal.amount.toLocaleString("de-DE"))}}</strong>
        </b-col>
      </b-row>
    </div>

    <div v-if="orders && orders.length <= 0" class="order-empty">
      <div class="content">
        <div>Agrega algo al carrito.
          <br>
          <br>
          <b-button @click="gotoDealPage()" class="primary-button">Llevame a la pagina de inicio</b-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import notification from '@/services/notificationService';

export default {
  name: 'OrderDetail',
  data() {
    return {
      countOptions: [],
    };
  },

  created() {
    this.countOptions = Array.from(Array(5).keys(), val => val + 1);
  },

  methods: {
    orderPicture(img) {
      return {
        'background-image': `url(${img})`,
        width: '100%',
        height: '70px',
        'background-size': 'contain',
        'background-repeat': 'no-repeat',
      };
    },

    gotoDealPage() {
      this.$router.push('/');
    },

    async gotoProduct(pid) {
      if (!pid) return;
      // await this.done();
      this.$router.push(`/products/${pid._id}`);
    },

    async updateCartItem(item) {
      this.$nextTick(async () => {
        if (item.counts > 0) {
          try {
            await this.$store.dispatch('cartStore/updateOrders', [item]);
            notification.success(this, 'El carrito se actualizo satisfactoriamente.');
          } catch (err) {
            console.log('Error', err);
            notification.error(
              this,
              'Cart could not be updated at the moment. Please try again later.',
            );
          }
        }
      });
    },

    async deleteSelected(item) {
      try {
        await this.$store.dispatch('cartStore/deleteOrders', [item]);
      } catch (err) {
        console.log(err);
      }
    },
  },

  filters: {
    customDisplay(val) {
      return val.indexOf('|') >= 0 ? val.split('|')[0] : val;
    },
  },

  computed: {
    ...mapGetters({
      orders: 'cartStore/getCart',
      cartTotal: 'cartStore/getTotal',
      subtotal: 'cartStore/getSubTotal',
      serviceCharge: 'cartStore/getServiceCharge',
      shippingPrice: 'cartStore/getShippingPrice',
      tariffPrice: 'cartStore/getTariffPrice',
      totalWeight: 'cartStore/getTotalWeight',
    }),
  },
};
</script>

<style lang="scss" scoped>
@import '../../assets/css/global.scss';

.edit-bag {
  font-size: 18px;
  margin-left: 20px;
  color: #bdbdbd;
  cursor: pointer;
}


.total-line {
  padding: 10px;
}
.order-detail {
  margin-top: 30px;
  .order-empty {
    height: 500px;
    line-height: 500px;
    color: #bdbdbd;
    font-size: 1.5em;
    text-align: center;

    .content {
      display: inline-block;
      vertical-align: middle;
      line-height: normal;
    }
  }

  .orders {
    list-style-type: none;
    padding: 0px;
    width: 100%;

    .order-desc {
      cursor: pointer;
    }

    .checkbox-item {
      padding-top: 20px;
    }

    li {
      padding: 10px;
      margin-bottom: 10px;
      width: 100%;

      &:nth-child(even) {
        background: #eeeeee;
      }
    }
  }
}
</style>
