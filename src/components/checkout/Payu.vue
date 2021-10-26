<template>
  <div class="order-detail align-left">
    <form
      method="post"      
      action="https://sandbox.checkout.payulatam.com/ppp-web-gateway-payu/"
      
      
    >
      <input name="merchantId" type="hidden" value="508029" />
      <input name="accountId" type="hidden" value="512321" />
      <input name="description" type="hidden" :value="checksname" />
      <input name="referenceCode" type="hidden" :value="checksname">{{checksname}}
      <input name="amount" type="hidden"  v-model="cartTotal.amount"/>
      <!-- v-model="cartTotal.amount" -->
      <input name="tax" type="hidden" value="0" />
      <input name="taxReturnBase" type="hidden" value="0" />
      <input name="currency" type="hidden" value="COP" />
      <input
        name="signature"
        type="hidden"
        :value="keysname"
      />
      <input name="test" type="hidden" value="1" />
      <input name="buyerEmail" type="hidden" :value="emailname"/>
      <!-- v-model="email.amount" -->
      <input
        name="responseUrl"
        type="hidden"
        value="http://localhost:5201/#/orders"
      />
      <input
        name="confirmationUrl"
        type="hidden"
        value="https://shopping-server-todopet.herokuapp.com/ui/payu"
      />

      <input name="shippingAddress" type="hidden" :value="adressname" >
      <!-- v-model="adress.amount" -->
      <input name="shippingCity" type="hidden" value="Bogota" >
      <input name="shippingCountry" type="hidden" value="CO" >
      <!-- <b-btn class="primary-button" @click="enviarPago()">Prueba boton</b-btn>&nbsp;&nbsp; -->

      <b-btn  @click="handlePayment()" formtarget="_blank" ref="Submit" type="submit">
        <img src="@/assets/images/PAYU.png" alt="">
      </b-btn>&nbsp;&nbsp;
      <!-- <input  @click="handlePayment()" name="Submit" type="submit" value="Enviar"  /> -->

    </form>
  </div>
</template>

<script>
import CryptoJS from 'crypto-js'
import { mapGetters } from 'vuex';
import notification from '@/services/notificationService';

export default {
  name: 'OrderDetail',
  data() {
    return {
      merchantId: '',
      accountId: '',
      description:'',
      tax:'',
      taxReturnBase:'',
      currency:'',
      test:'',
      responseUrl:'',
      confirmationUrl:'',
      key: '',
      merchant:'',
      referencia: '',
      firma:'',
      hash: ''
    };
  },

  created() {
  },

  methods: {


      async handlePayment() {
      try {        
        this.$refs.submit;
        await this.$store.dispatch('cartStore/pay');
        notification.success(this, 'Payment processed');
        this.shippingMethod = null;

      } catch (error) {
        console.log(error);
        const msg = error.httpStatus ? '' : error.response.data.errorDetails;
        notification.error(this, `Error: ${msg}`, 'all');
      }
    },  
    // enviarPago() {
    //   let formData = new URLSearchParams();
    //   formData.append("accountId", "512321");
    //   formData.append("merchantId", "508029");
    //   formData.append("description", "Compra en Pets Love");
    //   formData.append("tax", "0");
    //   formData.append("taxReturnBase", "0");
    //   formData.append("currency", "COP");
    //   formData.append("test", "1");

    //   formData.append("shippingCity", "Bogota");
    //   formData.append("shippingCountry", "CO");
    //   formData.append("responseUrl", "http://localhost:5201/#/orders");
    //   formData.append("confirmationUrl", "http://localhost:5201/#/orders");
    //   formData.append("shippingAddress", adressname());
    //   formData.append("referenceCode", checksname());
    //   formData.append("amount", cartTotal.amount);
    //   formData.append("signature", keysname());
    //   formData.append("buyerEmail", emailname());
    //   formData.append("shippingAddress", "CO");

    //   axios
    //     .post("https://sandbox.checkout.payulatam.com/ppp-web-gateway-payu/", formData, {
    //       headers: {
    //         "Access-Control-Allow_Methods": "POST",
    //       },
    //     })
    //     .then(
    //       console.log(formData)
          
    //     );
    // },
    // form(){
    //   console.log(formData)
    // }


  },
  computed: {
    ...mapGetters({
      adress: 'shippingStore/getSelectedAddress',
      email: 'authStore/getEmail',
      orders: 'cartStore/getCart',
      cartTotal: 'cartStore/getTotal',
      subtotal: 'cartStore/getSubTotal',
      serviceCharge: 'cartStore/getServiceCharge',
      shippingPrice: 'cartStore/getShippingPrice',
      tariffPrice: 'cartStore/getTariffPrice',
      totalWeight: 'cartStore/getTotalWeight',
      checkoutId: 'cartStore/checkoutId',
    }),
    emailname() {
      return this.$store.getters['authStore/getEmail'];
    },
    adressname() {
      return this.$store.getters['shippingStore/getSelectedAddress'];
    },
    checksname() {
      return this.$store.getters['cartStore/checkoutId'];
    },    
    keysname() {
        let monto = this.$store.getters['cartStore/getTotal']
        let total = monto.amount
        let referencia = this.$store.getters['cartStore/checkoutId']
        

        const hash = CryptoJS.MD5('4Vj8eK4rloUd272L48hsrarnUA'+'~'+'508029'+'~'+referencia+'~'+total+'~'+'COP')
      return hash;
    },    
  },
};
</script>

<style lang="scss" scoped>

</style>
