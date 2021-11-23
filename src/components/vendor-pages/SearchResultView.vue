<template>
  <div class="product-detail">
    <div class="space"></div>
    <br>
    <p class="align-left bcrumb">Comprar &nbsp; / &nbsp; {{title}}</p>
    <br>

    <!-- Design for mobile view -->
    <div class="d-block d-md-none d-none">
      <div class="d-flex flex-row justify-content-center align-items-center">
        <div>
          <b-dropdown
            text="Filtra aqui por categoria"
            block
            variant="success"
            class="m-md-2"

            menu-class="w-100"
          >
           <ul v-show="Object.keys(menu).length > 0">
             <li v-for="(product, pkey) in menu" v-bind:key="pkey">
              <div v-if="product && product.length > 0 && shouldProductDisplay(pkey, product)">
               <ul>
                <li v-for="(subcategory, sid) in product" v-bind:key="sid">
                  <a
                    @click="openSubCategory(subcategory, pkey)"
                    :class="{'bold': activeSubCategory(subcategory.subcategory)}"
                  >{{subcategory.subcategory}}</a>
                </li>
              </ul>
              </div>
            </li>
          </ul>               
          </b-dropdown>
        </div>
      </div>
    </div>
    <b-row>
      <b-col md="2" class="beginner align-left">
        <div class="d-none d-md-block">
          <side-menu-view
            :sidebar="menu"
            :category="category"
            :subCategory="subCategory"
            :term="term"
          ></side-menu-view>
        </div>
      </b-col>
      <b-col md="10">
        <div v-if="data && data.length > 0">
          <div class="product-card align-center" v-for="(product, pid) in data" v-bind:key="pid">
            <div class="link" @click="openProductDetail(product._id)">
              <div class="img-parent" v-if="product.thumbnailUrls.length > 0">
                <search-result-view-image :product="product"/>
              </div>

              <p
                v-else
                style="font-size: 5em; padding: 10px 0px; text-align: center; color: #bdbdbd"
              >
                <font-awesome-icon icon="shopping-bag" width="100%"/>
              </p>
              <div class="product-card-desc">
                <p class="info">{{product.store}}</p>
                <p class="title">{{product.name}}</p>
                <p>
                  <span
                    v-if="product.marked_price && product.marked_price.amount > product.price.amount"
                  >
                    <strong
                      class="underline"
                      style="color: red"
                    >$ {{product.marked_price.amount.toLocaleString("de-DE")}}</strong>&nbsp;&nbsp;
                  </span>

                  <strong>
                    <span>$ {{product.price.amount.toLocaleString("de-DE")}}</span>
                  </strong>
                </p>
              </div>
            </div>
          </div>
          <div class="align-center">
            <b-btn
              class="primary-button"
              @click="loadMoreProducts()"
              v-show="data.length < paging.total"
            >Ver mas</b-btn>
          </div>
        </div>
        <div v-else>
          <div class="info" style="font-size: 50px">No se encontraron productos</div>
        </div>
      </b-col>
    </b-row>
  </div>
</template>


<script>
import SearchResultViewImage from '@/components/vendor-pages/SearchResultViewImage.vue';
import SideMenuView from '@/components/vendor-pages/SideMenuView.vue';

export default {
  name: 'SearchResultView',
  props: {
    data: {
      type: Array,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    menu: {
      required: true,
    },
    term: {
      required: false,
      default: '',
      type: String,
    },

    category: {
      required: false,
      default: '',
      type: String,
    },

    subCategory: {
      required: false,
      default: '',
      type: String,
    },

    paging: {
      required: false,
      default: null,
      type: Object,
    },  
  },
  components: {
    SearchResultViewImage,
    SideMenuView,
  },

  methods: {
    getPictureStyle(img) {
      return {
        'background-image': `url(${img})`,
        'background-size': 'cover',
        'margin-bottom': '10px',
      };
    },

    openProductDetail(pid) {
      this.$router.push(`/products/${pid}`);
    },

    loadMoreProducts() {
      this.$emit('nextpage');
    },
    openSubCategory(subcat, keyy) {
      const q = {};
      if (this.term) q.term = this.term;
      q.category = this.category;
      q.subCategory = subcat.subcategory;
      this.$router.push({
        path: '/search',
        query: {
          term: this.term,
          category: this.category.length > 0 ? this.category : keyy,
          subCategory: subcat.subcategory,
        },
      });
    },

    shouldProductDisplay(catgry, subcats) {
      if (this.category.length <= 0) {
        if (
          this.subCategory.length > 0
          && _.findIndex(subcats, v => v.subcategory === this.subCategory) >= 0
        ) {
          return true;
        }
        if (this.subCategory.length <= 0) return true;
        return false;
      }
      if (this.category === catgry) return true;
      return false;
    },

    activeSubCategory(subCat) {
      return subCat === this.subCategory;
    },    
  },
  computed: {},  
};
</script>

<style lang="scss">
.search-result-view {
  text-align: left;
  width: 80%;
  margin: auto;
  padding: 30px 0px;
  padding-bottom: 3rem;
}

.result-view {
  margin-top: 1em;
}

.product-card {
  display: inline-block;
  // box-shadow: 3px 4px 5px 0px #ccc;
  // background-color: white;
  border-radius: 0px;
  margin: 20px 20px 30px 0px;
  width: 225px;
  // box-shadow: 0px 0 5px rgba(1, 41, 112, 0.08);
  // padding: 30px;
  // &:hover .img-cls {
  //   transform: scale(1.1);
  //   transition: all 0.5s;
  // }

  .img-parent {
    height: 300px;
    width: 225px;
    overflow: hidden;
  }

  .product-card-desc {
    margin-top: 0.5em;
    
    .title {
      height: 2em;
      text-overflow: ellipsis;
      overflow: hidden;
    }

    .price {
      font-weight: bold;
    }
  }

  .link {
    cursor: pointer;
  }
  p {
    padding: 3px 5px;
    margin: 0px;
  }
}
.bcrumb {
  font-size: 0.75em;
}
.product-detail {
  width: 90%;
  margin-left: auto;
  margin-right: auto;

  margin-bottom: 10px;
}
.boton-filtro {
  background-color: white; /*this for transparent button*/
  border: 2px solid black; /* this is for button border*/
  border-radius: 0px;
  color: black;
  padding: 10px 40px;
}
.boton-filtro:hover {
  background-color: black; /*this for transparent button*/
  border: 2px solid black; /* this is for button border*/
  border-radius: 0px;
  color: white;
}
</style>
