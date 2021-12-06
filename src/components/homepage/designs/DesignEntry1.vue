<template>
  <div id="design-entry">
    <!-- This previews design specific to the type of the design we have  -->
    <div v-if="design && design.config.design_type === 'Canvas Right Cracked'">     
      <canvas-crack-single-1
        :product="design.products[0]"
        :fecha="design.config.name"
        :type="1"
        @shop="shop"
      />     
    </div>
  </div>  
</template>

<script>
import StripeDesign from './StripeDesign.vue';
import CanvasCrackSingle1 from './CanvasCrackSingleDesign1.vue';

export default {
  props: {
    design: {
      type: Object,
      required: true,
    },
  },
  components: {
    StripeDesign,
    CanvasCrackSingle1,
  },

  methods: {
    shop(product) {
      this.$emit('shop', product);
    },
  },
};
</script>

<style lang="scss">
#design-entry {
  font-family: 'Quicksand', sans-serif;
}
</style>
