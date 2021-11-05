<template>
  <div>
    <!-- <div class="block news dark">
      <div class="newsletter dark">
        <b-row>
          <b-col class="align-right center-sm">
            <span>Get Notified of hottest deals
              <br>
              <input type="text" class="input" v-model="subscriptionEmail">
            </span>
          </b-col>
          <b-col class="align-left center-sm">
            <b-button
              class="subscribe-button veniqa-button dark"
              @click="subscribeClicked()"
            >Subscribe</b-button>
          </b-col>
        </b-row>
      </div>
    </div>-->
    <div class="block light">
      <div class="main-block">
        <b-row>
          <b-col class="align-left center-sm" md="4">
            <a href="/#/privacy">Politica de privacidad</a>
            <br>
            <a href="/#/terms">Terminos y condiciones de uso</a>
          </b-col>
          <b-col md="4">
            <div>
              <img src="@/assets/images/pago.png" alt="">
            </div>
            <div>
            @2021 Luka Petshop
            </div>
          </b-col>
          <b-col class="align-right center-sm" md="4">
            <a>Facebook</a>
            <br>
            <a href="https://www.instagram.com/luka.petshop2021/">Instagram</a>
          </b-col>
        </b-row>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Footer',
  data() {
    return {
      subscriptionEmail: '',
    };
  },
};
</script>

<style scoped lang="scss">
@import '../assets/css/global.scss';

.block {
  height: 250px;
  padding: 10px 0px;
  // margin: 10px 0px;

  line-height: 250px;

  span,
  .main-block {
    display: inline-block;
    vertical-align: middle;
    line-height: normal;
    width: 80%;
  }

  @media (max-width: 768px) {
    line-height: normal;
    .center-sm {
      text-align: center;
      margin: 15px 0px;
    }

    padding-top: 50px;
  }

  .newsletter {
    font-size: large;

    .input {
      width: 300px;
    }

    .subscribe-button {
      width: 220px;
      height: 60px;
      margin: 10px 0px;
    }
  }

  &.dark {
    background-color: $pitch-black;
    color: white;
  }

  .light {
    color: $pitch-black;
  }
}
</style>
