<template>
  <div>
    <!-- ======= Frequently Asked Questions Section ======= -->
    <section id="contenedor" class="faq">
      <div class="container">
        <div class="faq-list">
          <ul>
            <li>
              <a>¿Tienen cobertura en toda Bogota?</a>
              <div>
                <p>
                  Si, realizamos envios a toda la ciudad.
                </p>
              </div>
            </li>
            <hr />
            <li>
              <a>¿Aceptan pago contraentrega?</a>
              <div>
                <p>Por el momento no se acepta pago contraentrega.</p>
              </div>
            </li>
            <hr />
            <li>
              <a>¿Tienen tienda fisica?</a>
              <div>
                <p>
                  Somos una tienda en linea, pero cualquier duda que tengas la
                  podremos solucionar.
                </p>
              </div>
            </li>
            <hr />
            <li>
              <a>¿El envio es gratis?</a>
              <div>
                <p>Se ofrece envio gratis por compras superiores a $150.000.</p>
              </div>
            </li>
            <hr />
            <li>
              <a>¿En que plazo se realizan los envios?</a>
              <div>
                <p>
                  Puedes elegir el plazo de envio al finalizar la compra. A
                  partir del dia siguiente puedes escoger recibirlos.
                </p>
              </div>
            </li>
            <hr />
          </ul>
        </div>
      </div>
    </section>
    <!-- End Frequently Asked Questions Section -->
  </div>
</template>

<script>
/* eslint-disable prefer-destructuring */

export default {
  name: 'Faq',
};
</script>

<style scoped>
.container {
  position: relative;
  margin-left: auto;
  margin-right: auto;
  width: 80%;

}

.faq .faq-list ul {
  padding: 0;
  list-style: none;
}

.faq .faq-list li {
  padding: 20px;
  background: #fff;
  border-radius: 4px;
}
.faq .faq-list a {
  font-family: 'Poppins', sans-serif;
  font-size: 16px;
}
</style>
