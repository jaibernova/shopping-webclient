<template>
  <div class="featured-product">
    <!-- This is for screens bigger than MD -->
    <!-- <div class="type-separator d-none d-md-block">
      <b-row style="min-height:100%">
        <b-col md="5" v-if="type == 2">
          <div :style="productImageStyle" class="product-img product-clip-right"></div>
        </b-col>
        <b-col md="7">          
          <div class="product-content">
            <div class="product-body">
              <div class="body-content">
                
                <span class="brand">{{product.brand}}</span>
                <span class="name">{{product.name}}</span>
                <span class="amount">$ {{product.price.amount.toLocaleString("de-DE")}}</span>

                <b-button class="primary-button" @click="$emit('shop', product)">Ver producto</b-button>
              </div>
            </div>
          </div>
        </b-col>

        <b-col md="5" v-if="type === 1">
          <div :style="productImageStyle" class="product-img product-clip"></div>
        </b-col>
      </b-row>
    </div> -->

    <!-- Deal Of The Week Section Begin-->


    <div class="deal-of-week">
      <div class="container1">
        <div class="row">
          <div class="col-lg-6 d-none d-md-block">
           <!-- <a id="boton-deal3">Destacado</a> -->
            <div :style="productImageStyle" class="product-img"></div>
          </div>

          <div class="col-lg-6">
            <div class="section-title">
              <h2 class="product-price">
                {{ product.name }}
              </h2>
              <!-- <div>
                <span> {{ product.brand }}</span>
              </div> -->
              <div class="d-block d-md-none d-none">
                <div>
                  <a id="boton-deal2">Destacado</a>
                  <img
                    class="img-cls-featured"
                    :src="product.detailedImageUrls[0]"
                    alt="chunky cordero salmon"
                  />
                </div>
              </div>
              <br />
              <div class="product-price" >
                
               <a style="text-decoration: line-through"> Antes: $ {{product.marked_price.amount.toLocaleString("de-DE")}} </a>
              </div>
              <div class="product-price2">
               Ahora: $ {{ product.price.amount.toLocaleString('de-DE') }}
              </div>
            </div>
            <Countdown :date="end" @onFinish="finish()"></Countdown>
            <!-- <div class="countdown-timer" id="countdown"> -->
            <!-- <div class="cd-item">
                <span>56</span>
                <p>Days</p>
              </div>
              <div class="cd-item">
                <span>12</span>
                <p>Hrs</p>
              </div>
              <div class="cd-item">
                <span>40</span>
                <p>Mins</p>
              </div>
              <div class="cd-item">
                <span>52</span>
                <p>Secs</p>
              </div> -->
            <div>
              <br />
              <br />

              <a @click="$emit('shop', product)" id="boton-deal"
                >Ver producto</a
              >
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Deal Of The Week Section End -->
    <!-- End of design for screen bigger than md -->

    <!-- Design for mobile view -->
    <!-- <div class="d-block d-md-none d-none">
      <div class="d-flex flex-row justify-content-center align-items-center">
        <div
          sm="2"
          :style="productImageStyle"
          class="
            box
            d-flex
            flex-column
            justify-content-center
            align-items-center
          "
        >
          <div
            class="
              description
              d-flex
              flex-column
              justify-content-center
              align-items-center
            "
          >
            <span class="brand">{{ product.brand }}</span>
            <span class="name">{{ product.name }}</span>
            <span class="amount"
              >$ {{ product.price.amount.toLocaleString('de-DE') }}</span
            >

            <span class="button-like" @click="$emit('shop', product)"
              >Ver producto</span
            >
          </div>
        </div>
      </div>
    </div> -->
    <br />
  </div>
</template>

<script>
import Countdown from './Countdown.vue';
/* eslint-disable prefer-destructuring */

export default {
  components: { Countdown },
  name: 'CanvasCrackSingleDesign',
  props: {
    product: {
      required: true,
      type: Object,
    },
    type: {
      required: false,
      type: Number,
      default: 1,
    },
    fecha: {
      required: true,
    },
  },
  data() {
    return {
      displayImage: null,
      productClass: null,
      end: new Date(this.fecha),
    };
  },
  methods: {
    finish() {
      console.log('finish');
    },
  },
  created() {
    if (
      this.product &&
      this.product.detailedImageUrls &&
      this.product.detailedImageUrls.length > 0
    ) {
      this.displayImage = this.product.detailedImageUrls[0];
    }
  },

  watch: {
    product() {
      this.displayImage = this.product.detailedImageUrls[0];
    },
  },

  computed: {
    productImageStyle() {
      return {
        'background-image': `url(${this.displayImage})`,
        'background-size': 'contain',
        'background-repeat': 'no-repeat',
      };
    },
    productImageStyle2() {
      return {
        'background-image': `url(${this.displayImage})`,
        'background-size': '100%',
        'background-repeat': 'no-repeat',
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.product-img {
  height: 110%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.img-cls-featured {
  border-top-right-radius: 0.25rem;
  border-top-left-radius: 0.25rem;
  height: auto;
  width: 100%;
}
// .featured-product {
//   height: 90vh;
//   padding: 20px 0px;

//   .button-like {
//     font-size: large;
//     font-weight: bold;
//     margin-top: 2rem;
//     padding: 1rem 2rem;
//     border: 1px solid transparent;
//   }

//   .button-like:hover {
//     border: 1px solid white;
//     background: white;
//     color: black;
//   }

//   .box {
//     height: 90vh;
//     width: 100%;
//     background-color: white;
//     background-size: cover;
//     background-position: center center;
//     padding: 0px 0px 0px 0px !important;
//   }
//   .box:hover {
//     background-size: cover;
//     background-position: center center;
//   }
//   .description {
//     height: inherit;
//     width: inherit;
//     color: rgba(0, 0, 0, 0);
//   }
//   .description:hover {
//     color: white;
//     background-color: #1b1a1a66;
//     -webkit-transition: background-color 200ms linear;
//     -ms-transition: background-color 200ms linear;
//     transition: background-color 200ms linear;
//   }

//   .product-clip {
//     -webkit-clip-path: polygon(0 0, 100% 0, 100% 100%, 13% 100%);
//     clip-path: polygon(0 0, 100% 0, 100% 100%, 13% 100%);

//     background-repeat: no-repeat;
//     background-size: cover;
//     background-position: right;
//   }

//   .product-clip-right {
//     -webkit-clip-path: polygon(0 0, 100% 0, 90% 100%, 0% 100%);
//     clip-path: polygon(0 0, 100% 0, 90% 100%, 0% 100%);

//     background-repeat: no-repeat;
//     background-size: cover;
//     background-position: left;
//   }

//   .type-separator {
//     height: 100%;
//   }

//   .product-content {
//     display: table;
//     height: 100%;
//     width: 100%;

//     .product-body {
//       display: table-row;
//       .body-content {
//         display: table-cell;
//         vertical-align: middle;
//         padding-left: 3rem;
//         padding-right: 1.5rem;

//         &.content-prop {
//           font-size: 2em;
//         }

//         span {
//           display: block;
//           font-size: 1.3em;
//           padding: 10px 0px;
//         }

//         button {
//           padding: 10px 50px;
//         }

//         .brand {
//           font-weight: bold;
//         }
//       }

//       .image {
//         height: 100px;
//         width: 100%;
//       }
//     }
//   }
// }
/*---------------------
  Deal Of The Week
-----------------------*/

.deal-of-week {
  background-color: #f8f3fd;
  padding-top: 40px;
  padding-bottom: 70px;
  margin-left: 15px;
  margin-right: 15px;
}

.countdown-timer {
  text-align: center;
  margin-bottom: 50px;
}

.set-bg {
  background-repeat: no-repeat;
  background-size: cover;
  background-position: top center;
}

.spad {
  padding-top: 100px;
  padding-bottom: 100px;
}

.product-price {
  font-size: 17px;
  font-weight: 700;
  color: #40176e;
}
.product-price2 {
  font-size: 20px;
  font-weight: 700;
  color: #40176e;
}

.product-price span {
  font-size: 16px;
  font-weight: 400;
  color: #636363;
}
.img-fluid {
  max-width: 100%;
  height: auto;
}

#boton-deal {
  display: inline-block;
  font-size: 14px;
  font-weight: 700;
  padding: 10px 15px;
  color: #ffffff;
  background: #e7ab3c;
  text-transform: uppercase;
  text-decoration: none;
}
#boton-deal2 {
  margin-right: 0%;
  display: inline-block;
  font-size: 14px;
  font-weight: 700;
  padding: 5px 10px;
  color: #ffffff;
  background: #1ed145;
  text-transform: uppercase;
  text-decoration: none;
}
#boton-deal3 {
  margin-right: 19%;
  display: inline-block;
  font-size: 15px;
  font-weight: 700;
  padding: 5px 35px;
  color: #ffffff;
  background: #1ed145;
  text-transform: uppercase;
  text-decoration: none;
}

.container1 {
  margin-left: 25%;
  margin-right: 25%;
}
</style>
