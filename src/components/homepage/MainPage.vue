<template>
  <div id="main-page">
    <!-- <div class="main-bg"></div> -->
    <div>
      <carousel />
    </div>
    <br />
    <br />
    <div>
      <br />
      <div class="section-title">
        <h2>Mas vendidos</h2>
      </div>

      <br />
    </div>

    <div v-for="(design, did) in featuredDesigns" :key="did" class="designs">
      <design-entry :design="design" @shop="gotoProduct" />
    </div>
    <br />
    <br />
    <div class="section-title">
      <h2>¿Porque escogernos?</h2>
    </div>
    <div id="why-us" class="why-us">
      <div id="container">
        <div class="section-title">
          <!-- <p>Ut possimus qui ut temporibus culpa velit eveniet modi omnis est adipisci expedita at voluptas atque vitae autem.</p> -->
        </div>

        <div class="row">
          <div class="col-lg-3">
            <div class="box">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="40"
                height="40"
                fill="currentColor"
                class="bi bi-truck"
                viewBox="0 0 16 16"
              >
                <path
                  d="M0 3.5A1.5 1.5 0 0 1 1.5 2h9A1.5 1.5 0 0 1 12 3.5V5h1.02a1.5 1.5 0 0 1 1.17.563l1.481 1.85a1.5 1.5 0 0 1 .329.938V10.5a1.5 1.5 0 0 1-1.5 1.5H14a2 2 0 1 1-4 0H5a2 2 0 1 1-3.998-.085A1.5 1.5 0 0 1 0 10.5v-7zm1.294 7.456A1.999 1.999 0 0 1 4.732 11h5.536a2.01 2.01 0 0 1 .732-.732V3.5a.5.5 0 0 0-.5-.5h-9a.5.5 0 0 0-.5.5v7a.5.5 0 0 0 .294.456zM12 10a2 2 0 0 1 1.732 1h.768a.5.5 0 0 0 .5-.5V8.35a.5.5 0 0 0-.11-.312l-1.48-1.85A.5.5 0 0 0 13.02 6H12v4zm-9 1a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm9 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2z"
                />
              </svg>
              <h4>Envios seguros</h4>
              <p>Si lo quieres, en 24 horas recibes tu pedido.</p>
            </div>
          </div>

          <div class="col-lg-3">
            <div class="box">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="40"
                height="40"
                fill="currentColor"
                class="bi bi-credit-card"
                viewBox="0 0 16 16"
              >
                <path
                  d="M0 4a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4zm2-1a1 1 0 0 0-1 1v1h14V4a1 1 0 0 0-1-1H2zm13 4H1v5a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1V7z"
                />
                <path
                  d="M2 10a1 1 0 0 1 1-1h1a1 1 0 0 1 1 1v1a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1v-1z"
                />
              </svg>
              <h4>Facilidades de pago</h4>
              <p>Paga con multiples medios de pago gracias a PayU.</p>
            </div>
          </div>

          <div class="col-lg-3">
            <div class="box">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="40"
                height="40"
                fill="currentColor"
                class="bi bi-lock"
                viewBox="0 0 16 16"
              >
                <path
                  d="M8 1a2 2 0 0 1 2 2v4H6V3a2 2 0 0 1 2-2zm3 6V3a3 3 0 0 0-6 0v4a2 2 0 0 0-2 2v5a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2zM5 8h6a1 1 0 0 1 1 1v5a1 1 0 0 1-1 1H5a1 1 0 0 1-1-1V9a1 1 0 0 1 1-1z"
                />
              </svg>
              <h4>Pagos seguros</h4>
              <p>Tu compra esta protegida.</p>
            </div>
          </div>

          <div class="col-lg-3">
            <div class="box">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="40"
                height="40"
                fill="currentColor"
                class="bi bi-tags"
                viewBox="0 0 16 16"
              >
                <path
                  d="M3 2v4.586l7 7L14.586 9l-7-7H3zM2 2a1 1 0 0 1 1-1h4.586a1 1 0 0 1 .707.293l7 7a1 1 0 0 1 0 1.414l-4.586 4.586a1 1 0 0 1-1.414 0l-7-7A1 1 0 0 1 2 6.586V2z"
                />
                <path
                  d="M5.5 5a.5.5 0 1 1 0-1 .5.5 0 0 1 0 1zm0 1a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3zM1 7.086a1 1 0 0 0 .293.707L8.75 15.25l-.043.043a1 1 0 0 1-1.414 0l-7-7A1 1 0 0 1 0 7.586V3a1 1 0 0 1 1-1v5.086z"
                />
              </svg>
              <h4>Precios bajos</h4>
              <p>Nos esforzamos por traerte las mejores tarifas.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br />
    <br />
    <br />
    <br />
    <div class="section-title">
      <h2>Clientes felices</h2>
    </div>
    <br />
    <br />
    <featured-stripe />
    <br />
    <br />
    <br/>
    <br/>
    <div class="section-title">
      <h2>Preguntas frecuentes</h2>
    </div>

    
    <div>
    <faq />
    </div>
  </div>
</template>

<script>
import Carousel from '@/components/homepage/Carousel.vue';
import Faq from '@/components/homepage/Faq.vue';
import Product from '@/data/featuredProduct.json';
import FeatureService from '@/services/FeaturedService';
import FeaturedStripe from '@/components/homepage/FeaturedStripe.vue';
import DesignEntry from '@/components/homepage/designs/DesignEntry.vue';

export default {
  name: 'MainPage',
  components: {
    FeaturedStripe,
    DesignEntry,
    Carousel,
    Faq,
  },

  data() {
    return {
      product: null,
      currentSection: 'homepage',
      featuredDesigns: [],
    };
  },

  methods: {
    gotoProduct(product) {
      this.$router.push(`/products/${product._id}`);
    },
  },

  created() {
    this.product = Product;
    FeatureService.getFeatureListFor(this.currentSection)
      .then((data) => {
        this.featuredDesigns = data;
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>

<style lang="scss">
@import '../../assets/css/global.scss';

.designs {
  margin-bottom: 1rem;
}
.veniqa-button:hover {
  background-color: $pitch-black;
  border: 2px solid $pitch-black;
}
.absolute-buttons {
  position: absolute;
  bottom: 0;
  width: 100%;
  margin-bottom: 100px;
}

.main-bg {
  // background-image: url('https://www.fearlesss.co.uk/media/wysiwyg/Copy_of_SPRING_TWENTY_NINETEEN-2.jpg');
  background-image: url('./../../assets/images/homepage.jpg');
  height: 100vh;
  background-size: cover;
  background-repeat: no-repeat;
  background-position-x: center;

  .content {
    margin-top: 100px;
    display: flex;
    height: 100%;
    margin: auto;

    div {
      margin: auto;
    }
  }
}

.jumbotron {
  padding: 0px 0px 0px 0px !important;
  margin: 0px 0px 0px 0px !important;
}
.featured-title {
  margin: 0px 0px 0px 0px;
  padding: 40px 10px;
  color: #5d6d7e;
  // background-color: #eaecee;
  font-family: 'Quicksand';
}


#why-us .box {
  padding: 50px 30px;
  box-shadow: 0px 2px 15px rgba(0, 0, 0, 0.1);
  transition: all ease-in-out 0.3s;
  height: 100%;
}
#why-us .box span {
  display: block;
  font-size: 28px;
  font-weight: 700;
  color: #94289e;
}
.why-us .box h4 {
  font-size: 24px;
  font-weight: 600;
  padding: 0;
  margin: 20px 0;
  color: #6c665c;
}
.why-us .box p {
  color: #aaaaaa;
  font-size: 15px;
  margin: 0;
  padding: 0;
}
.why-us .box:hover {
  background: #8038aa;
  padding: 30px 30px 70px 30px;
  box-shadow: 10px 15px 30px rgba(0, 0, 0, 0.18);
}
.why-us .box:hover span,
.why-us .box:hover h4,
.why-us .box:hover p {
  color: #fff;
}

.section-title {
  text-align: center;
  padding-bottom: 30px;
}
.section-title h2 {
  font-size: 35px;
  font-weight: bold;
  text-transform: uppercase;
  margin-bottom: 20px;
  padding-bottom: 20px;
  position: relative;
  color: #37517e;
}
.section-title h2::before {
  content: '';
  position: absolute;
  display: block;
  width: 120px;
  height: 1px;
  background: #ddd;
  bottom: 1px;
  left: calc(50% - 60px);
}
.section-title h2::after {
  content: '';
  position: absolute;
  display: block;
  width: 40px;
  height: 3px;
  background: #9847e4;
  bottom: 0;
  left: calc(50% - 20px);
}
.section-title p {
  margin-bottom: 0;
}
</style>
