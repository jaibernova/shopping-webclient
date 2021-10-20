<template>
  <div class="order-detail align-left">
    <form
      method="post"
      action="https://sandbox.checkout.payulatam.com/ppp-web-gateway-payu/"
    >
      <input name="merchantId" type="hidden" value="508029" />
      <input name="accountId" type="hidden" value="512321" />
      <input name="description" type="hidden" value="Compra en Pets Love" />
      <input name="referenceCode" type="hidden" value="TestPayU" />
      <input name="amount" type="hidden" v-model="tariffPrice.amount" />
      <input name="tax" type="hidden" value="3193" />
      <input name="taxReturnBase" type="hidden" value="16806" />
      <input name="currency" type="hidden" value="COP" />
      <input
        name="signature"
        type="hidden"
        value="7ee7cf808ce6a39b17481c54f2c57acc"
      />
      <input name="test" type="hidden" value="0" />
      <input name="buyerEmail" type="hidden" value="test@test.com" />
      <input
        name="responseUrl"
        type="hidden"
        value="http://www.test.com/response"
      />
      <input
        name="confirmationUrl"
        type="hidden"
        value="http://www.test.com/confirmation"
      />
      <input name="Submit" type="submit" value="Enviar" />
    </form>
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
    this.countOptions = Array.from(Array(200).keys(), (val) => val + 1);
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
            notification.success(
              this,
              'The cart has been successfully updated.'
            );
          } catch (err) {
            console.log('Error', err);
            notification.error(
              this,
              'Cart could not be updated at the moment. Please try again later.'
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
