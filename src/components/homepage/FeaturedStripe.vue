<template>
  <b-jumbotron class="stripecontent">
    <b-row class="d-flex flex-row justify-content-center align-items-center">
      <b-col
        sm="2"
        v-for="(product, key) in products"
        v-bind:key="key"
        v-bind:style="{ backgroundImage: 'url(' + product.url + ')' }"
        class="box d-flex flex-column justify-content-center align-items-center"
        href="#"
      >
        <div class="description d-flex flex-column justify-content-center align-items-center">
          <h4>{{product.name}}</h4>
          <p>{{product.price}}</p>
        </div>
      </b-col>
    </b-row>
  </b-jumbotron>
</template>

<script>
export default {
  data() {
    return {
      products: [
        {
          name: 'Max',
          price: 'Feliz con su Chunky',
          url: 'https://www.minuto30.com/wp-content/uploads/2018/07/perro-pitbull-3.jpg',
        },
        {
          name: 'Lulu',
          price: 'Solo come su Dog Chow',
          url: 'https://files.rcnradio.com/public/styles/image_834x569/public/2018-09/perro_0.jpg?itok=_NyGzhmE',
        },
        {
          name: 'Trixie',
          price: 'Le gusta mucho su Cat Show',
          url: 'https://www.avisos-colombia.com/uimages/colombia/12/37/5512/1.jpg',
        },
        {
          name: 'Tommy y Coco',
          price: 'No le puede faltar su comida humeda',
          url:   'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRVTeZQf-ybNlDWNXcf2ajm9-eOsGbc-iDQzA&usqp=CAU',
        },
        {
          name: 'Felix',
          price: 'Los enlatados son sus favoritos',
          url: 'https://petsfriendscol.files.wordpress.com/2019/05/whatsapp-image-2019-05-07-at-9.30.23-pm-2.jpeg',
        },
        {
          name: 'Lucas',
          price: 'Un bulto no le dura todo el mes',
          url: 'https://de10.com.mx/sites/default/files/styles/imagen_body/public/2020/02/10/bosque_aragon_parque_canino.jpg?itok=XVIyatms',
        },
      ],
    };
  },
};
</script>

<style lang="scss" scoped>
.box {
  height: 50vh;
  background-color: white;
  background-size: cover;
  background-position: center center;
  padding: 0px 0px 0px 0px !important;
}
.box:hover {
  background-size: cover;
  background-position: center center;
}
.description {
  height: inherit;
  width: inherit;
  color: rgba(0, 0, 0, 0);
}
.description:hover {
  color: white;
  background-color: #1b1a1aad;
  -webkit-transition: background-color 500ms linear;
  -ms-transition: background-color 500ms linear;
  transition: background-color 500ms linear;
}
</style>
