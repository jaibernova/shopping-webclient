<template>
<div>
    <VueSlickCarousel :arrows="true" :autoplay="true" :autoplaySpeed="6000"  :dots="true">
      <div>
         <section id="hero">
           <div class="hero-container">
              <div id="heroCarousel" class="carousel slide carousel-fade" data-bs-ride="carousel" data-bs-interval="5000">
                <ol class="carousel-indicators" id="hero-carousel-indicators"></ol>
                  <div class="carousel-inner" role="listbox">

                    <!-- Slide 1 -->
                    <div class="carousel-item active" >
                      <div class="carousel-container">
                        <div class="carousel-content container">
                          <h2 class="animate__animated animate__fadeInDown">Siguenos en instagram</h2>
                          <p class="animate__animated animate__fadeInUp">Para estar al tanto de todos nuestros nuevos productos</p>
                          <a href="#about" class="btn-get-started animate__animated animate__fadeInUp scrollto">Read More</a>
                        </div>
                      </div>
                    </div>
                  </div>
               </div>
              </div>
          </section>
      </div>
      <div>
         <section id="hero">
           <div class="hero-container">
              <div id="heroCarousel" class="carousel slide carousel-fade" data-bs-ride="carousel" data-bs-interval="5000">
                <ol class="carousel-indicators" id="hero-carousel-indicators"></ol>
                  <div class="carousel-inner" role="listbox">

                    <!-- Slide 1 -->
                    <div class="carousel-item2 active" >
                      <div class="carousel-container">
                        <div class="carousel-content container">
                          <h2 class="animate__animated animate__fadeInDown">¿Tienes dudas?</h2>
                          <p class="animate__animated animate__fadeInUp">¿O quieres pagar contraentrega? Haz click en el logo de whatsapp que se encuentra en la parte inferior derecha de la pagina. Te responderemos al instante </p>
                          <a href="#about" class="btn-get-started animate__animated animate__fadeInUp scrollto">Read More</a>
                        </div>
                      </div>
                    </div>
                  </div>
               </div>
              </div>
          </section>
      </div>
      <div>
         <section id="hero">
           <div class="hero-container">
              <div id="heroCarousel" class="carousel slide carousel-fade" data-bs-ride="carousel" data-bs-interval="5000">
                <ol class="carousel-indicators" id="hero-carousel-indicators"></ol>
                  <div class="carousel-inner" role="listbox">

                    <!-- Slide 1 -->
                    <div class="carousel-item3 active" >
                      <div class="carousel-container">
                        <div class="carousel-content container">
                          <h2 class="animate__animated animate__fadeInDown">Compra y recibe al instante</h2>
                          <p class="animate__animated animate__fadeInUp">Agrega tus productos al carrito y selecciona el tiempo de entrega que prefieras</p>
                          <a href="#/login?previousPath=%2F" class="btn-get-started animate__animated animate__fadeInUp scrollto">Inicia sesion</a>
                        </div>
                      </div>
                    </div>
                  </div>
               </div>
              </div>
          </section>
      </div>
    </VueSlickCarousel>  
</div>
</template>

<script>
  import VueSlickCarousel from 'vue-slick-carousel'
  import 'vue-slick-carousel/dist/vue-slick-carousel.css'
  // optional style for arrows & dots
  import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'

 
  export default {
    name: 'MyComponent',
    components: { VueSlickCarousel },
  } 
</script>


<style lang="scss">
#hero {
  width: 100%;
  height: calc(100vh - 110px);
  padding: 0;
  overflow: hidden;
  background: #000;
  
}
#hero .carousel-item {
  width: 100%;
  height: calc(100vh - 110px);
  background-size: cover;
  background-position: top right;
  background-repeat: no-repeat;
  overflow: hidden;
  background-image: url('https://enricgomez.b-cdn.net/wp-content/uploads/2020/10/portada-insatgram.jpg.webp');
        // background-image: url('https://image.freepik.com/foto-gratis/mujer-mostrando-icono-whatsapp-messenger_53876-41312.jpg');
}
#hero .carousel-item2 {
  width: 100%;
  height: calc(100vh - 110px);
  background-size: cover;
  background-position: top right;
  background-repeat: no-repeat;
  overflow: hidden;
  // background-image: url('https://enricgomez.b-cdn.net/wp-content/uploads/2020/10/portada-insatgram.jpg.webp');
  background-image: url('https://image.freepik.com/foto-gratis/mujer-mostrando-icono-whatsapp-messenger_53876-41312.jpg');
}
#hero .carousel-item3 {
  width: 100%;
  height: calc(100vh - 110px);
  background-size: cover;
  background-position: top right;
  background-repeat: no-repeat;
  overflow: hidden;
    background-image: url('https://img.huffingtonpost.com/asset/5fec98c426000017057a3581.jpeg?ops=scalefit_720_noupscale');
}
#hero .carousel-item::before {
  content: "";
  background-color: rgba(13, 30, 45, 0.6);
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  overflow: hidden;
}
#hero .carousel-item2::before {
  content: "";
  background-color: rgba(13, 30, 45, 0.6);
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  overflow: hidden;
}
#hero .carousel-item3::before {
  content: "";
  background-color: rgba(13, 30, 45, 0.6);
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  overflow: hidden;
}
#hero .carousel-container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  bottom: 0;
  top: 0;
  left: 0;
  right: 0;
  overflow: hidden;
}
#hero .carousel-content {
  text-align: left;
}
@media (max-width: 992px) {
  #hero, #hero .carousel-item {
    height: calc(100vh - 70px);
  }
  #hero .carousel-content.container {
    padding: 0 50px;
  }
}
@media (max-width: 992px) {
  #hero, #hero .carousel-item2 {
    height: calc(100vh - 70px);
  }
  #hero .carousel-content.container {
    padding: 0 50px;
  }
}
@media (max-width: 992px) {
  #hero, #hero .carousel-item3 {
    height: calc(100vh - 70px);
  }
  #hero .carousel-content.container {
    padding: 0 50px;
  }
}
#hero h2 {
  color: #fff;
  margin-bottom: 30px;
  font-size: 48px;
  font-weight: 900;
}
#hero p {
  width: 80%;
  -webkit-animation-delay: 0.4s;
  animation-delay: 0.4s;
  color: #fff;
}
#hero .carousel-inner .carousel-item {
  transition-property: opacity;
  background-position: center top;
}
#hero .carousel-inner .carousel-item2 {
  transition-property: opacity;
  background-position: center top;
}
#hero .carousel-inner .carousel-item3 {
  transition-property: opacity;
  background-position: center top;
}
#hero .carousel-inner .carousel-item,
#hero .carousel-inner .active.carousel-item-start,
#hero .carousel-inner .active.carousel-item-end {
  opacity: 0;
}
#hero .carousel-inner .active,
#hero .carousel-inner .carousel-item-next.carousel-item-start,
#hero .carousel-inner .carousel-item-prev.carousel-item-end {
  opacity: 1;
  transition: 0.5s;
}
#hero .carousel-inner .carousel-item-next,
#hero .carousel-inner .carousel-item-prev,
#hero .carousel-inner .active.carousel-item-start,
#hero .carousel-inner .active.carousel-item-end {
  left: 0;
  transform: translate3d(0, 0, 0);
}
#hero .carousel-control-prev, #hero .carousel-control-next {
  width: 10%;
}
#hero .carousel-control-next-icon, #hero .carousel-control-prev-icon {
  background: none;
  font-size: 48px;
  line-height: 1;
  width: auto;
  height: auto;
}
#hero .carousel-indicators li {
  cursor: pointer;
}
#hero .btn-get-started {
  font-family: "Raleway", sans-serif;
  font-weight: 500;
  font-size: 14px;
  letter-spacing: 1px;
  display: inline-block;
  padding: 12px 32px;
  border-radius: 5px;
  transition: 0.5s;
  line-height: 1;
  margin: 10px;
  color: #fff;
  -webkit-animation-delay: 0.8s;
  animation-delay: 0.8s;
  border: 0;
  background: #428bca;
}
#hero .btn-get-started:hover {
  background: #1c5c93;
}
@media (max-width: 768px) {
  #hero h2 {
    font-size: 28px;
  }
}
@media (max-height: 500px) {
  #hero, #hero .carousel-item {
    height: 120vh;
  }
}

@media (max-height: 500px) {
  #hero, #hero .carousel-item2 {
    height: 120vh;
  }
}
@media (min-width: 1024px) {
  #hero p {
    width: 60%;
  }
  #hero .carousel-control-prev, #hero .carousel-control-next {
    width: 5%;
  }
}


// .carousel {
//   position: relative;
// }

// .carousel.pointer-event {
//   touch-action: pan-y;
// }

// .carousel-inner {
//   position: relative;
//   width: 100%;
//   overflow: hidden;
// }
// .carousel-inner::after {
//   display: block;
//   clear: both;
//   content: "";
// }

// .carousel-item {
//   position: relative;
//   display: none;
//   float: left;
//   width: 100%;
//   margin-right: -100%;
//   -webkit-backface-visibility: hidden;
//   backface-visibility: hidden;
//   transition: transform 0.6s ease-in-out;
// }
// @media (prefers-reduced-motion: reduce) {
//   .carousel-item {
//     transition: none;
//   }
// }

// .carousel-item.active,
// .carousel-item-next,
// .carousel-item-prev {
//   display: block;
// }

// /* rtl:begin:ignore */
// .carousel-item-next:not(.carousel-item-start),
// .active.carousel-item-end {
//   transform: translateX(100%);
// }

// .carousel-item-prev:not(.carousel-item-end),
// .active.carousel-item-start {
//   transform: translateX(-100%);
// }

// /* rtl:end:ignore */
// .carousel-fade .carousel-item {
//   opacity: 0;
//   transition-property: opacity;
//   transform: none;
// }
// .carousel-fade .carousel-item.active,
// .carousel-fade .carousel-item-next.carousel-item-start,
// .carousel-fade .carousel-item-prev.carousel-item-end {
//   z-index: 1;
//   opacity: 1;
// }
// .carousel-fade .active.carousel-item-start,
// .carousel-fade .active.carousel-item-end {
//   z-index: 0;
//   opacity: 0;
//   transition: opacity 0s 0.6s;
// }
// @media (prefers-reduced-motion: reduce) {
//   .carousel-fade .active.carousel-item-start,
// .carousel-fade .active.carousel-item-end {
//     transition: none;
//   }
// }

// .carousel-control-prev,
// .carousel-control-next {
//   position: absolute;
//   top: 0;
//   bottom: 0;
//   z-index: 1;
//   display: flex;
//   align-items: center;
//   justify-content: center;
//   width: 15%;
//   padding: 0;
//   color: #fff;
//   text-align: center;
//   background: none;
//   border: 0;
//   opacity: 0.5;
//   transition: opacity 0.15s ease;
// }
// @media (prefers-reduced-motion: reduce) {
//   .carousel-control-prev,
// .carousel-control-next {
//     transition: none;
//   }
// }
// .carousel-control-prev:hover, .carousel-control-prev:focus,
// .carousel-control-next:hover,
// .carousel-control-next:focus {
//   color: #fff;
//   text-decoration: none;
//   outline: 0;
//   opacity: 0.9;
// }

// .carousel-control-prev {
//   left: 0;
// }

// .carousel-control-next {
//   right: 0;
// }

// .carousel-control-prev-icon,
// .carousel-control-next-icon {
//   display: inline-block;
//   width: 2rem;
//   height: 2rem;
//   background-repeat: no-repeat;
//   background-position: 50%;
//   background-size: 100% 100%;
// }

// /* rtl:options: {
//   "autoRename": true,
//   "stringMap":[ {
//     "name"    : "prev-next",
//     "search"  : "prev",
//     "replace" : "next"
//   } ]
// } */
// .carousel-control-prev-icon {
//   background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23fff'%3e%3cpath d='M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z'/%3e%3c/svg%3e");
// }

// .carousel-control-next-icon {
//   background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23fff'%3e%3cpath d='M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z'/%3e%3c/svg%3e");
// }

// .carousel-indicators {
//   position: absolute;
//   right: 0;
//   bottom: 0;
//   left: 0;
//   z-index: 2;
//   display: flex;
//   justify-content: center;
//   padding: 0;
//   margin-right: 15%;
//   margin-bottom: 1rem;
//   margin-left: 15%;
//   list-style: none;
// }
// .carousel-indicators [data-bs-target] {
//   box-sizing: content-box;
//   flex: 0 1 auto;
//   width: 30px;
//   height: 3px;
//   padding: 0;
//   margin-right: 3px;
//   margin-left: 3px;
//   text-indent: -999px;
//   cursor: pointer;
//   background-color: #fff;
//   background-clip: padding-box;
//   border: 0;
//   border-top: 10px solid transparent;
//   border-bottom: 10px solid transparent;
//   opacity: 0.5;
//   transition: opacity 0.6s ease;
// }
// @media (prefers-reduced-motion: reduce) {
//   .carousel-indicators [data-bs-target] {
//     transition: none;
//   }
// }
// .carousel-indicators .active {
//   opacity: 1;
// }

// .carousel-caption {
//   position: absolute;
//   right: 15%;
//   bottom: 1.25rem;
//   left: 15%;
//   padding-top: 1.25rem;
//   padding-bottom: 1.25rem;
//   color: #fff;
//   text-align: center;
// }

// .carousel-dark .carousel-control-prev-icon,
// .carousel-dark .carousel-control-next-icon {
//   filter: invert(1) grayscale(100);
// }
// .carousel-dark .carousel-indicators [data-bs-target] {
//   background-color: #000;
// }
// .carousel-dark .carousel-caption {
//   color: #000;
// }

</style>
