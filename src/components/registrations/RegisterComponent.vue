<template>
  <div class="white-bg">
    <div class="header">
      <h2>
        <strong>Registrate</strong>
      </h2>
    </div>

    <b-form-group>
      <b-form-input
        id="name"
        type="text"
        name="name"
        :state="nameState"
        v-model="name"
        placeholder="Ingresa tu nombre"
        aria-describedby="nameFeedback"
      ></b-form-input>
      <b-form-invalid-feedback id="nameFeedback" class="align-left">
        <!-- This will only be shown if the preceeding input has an invalid state -->
        El campo nombre no puede quedar vacio.
      </b-form-invalid-feedback>
    </b-form-group>

    <b-form-group>
      <b-form-input
        id="username"
        type="email"
        name="username"
        :state="usernameState"
        v-model="username"
        placeholder="Ingresa tu correo electronico"
        aria-describedby="usernameFeedback"
      ></b-form-input>
      <b-form-invalid-feedback id="usernameFeedback" class="align-left">
        <!-- This will only be shown if the preceeding input has an invalid state -->
        Ingresa una direccion de correo valida
      </b-form-invalid-feedback>
    </b-form-group>

    <b-form-group>
      <b-form-input
        id="password"
        type="password"
        name="password"
        :state="passwordState"
        v-model="password"
        placeholder="Ingresa una contraseña"
        aria-describedby="passwordFeedback"
      ></b-form-input>
      <b-form-invalid-feedback
        id="passwordFeedback"
        class="align-left">Ingresa al menos 7 caracteres.</b-form-invalid-feedback>
    </b-form-group>

    <b-form-group>
      <b-form-input
        id="confirmPassword"
        type="password"
        name="confirmPassword"
        :state="confirmPasswordState"
        v-model="confirmPassword"
        placeholder="Confirma tu contraseña"
        aria-describedby="confirmPasswordFeedback"
      ></b-form-input>
      <b-form-invalid-feedback
        id="confirmPasswordFeedback"
        class="align-left"
      >Las contraseñas deben coincidir.</b-form-invalid-feedback>
    </b-form-group>

    <b-form-group>
      <b-form-input
        id="phone"
        type="tel"
        name="phone"
        :state="phoneState"
        v-model="phone"
        placeholder="Ingresa tu numero celular"
        aria-describedby="phoneFeedback"
      ></b-form-input>
      <b-form-invalid-feedback
        id="phoneFeedback"
        class="align-left">Numero de celular invalido.</b-form-invalid-feedback>
    </b-form-group>

    <vue-recaptcha @verify="onVerify" @expired="onExpired" :sitekey="recaptchaKey"></vue-recaptcha>
    <p class="info align-left">Porfavor resuelve el captcha antes de iniciar sesion.</p>

    <div class="modal-bottom"></div>
    <b-btn class="register-button" @click="registerClicked()">Registrar</b-btn>

    <p class="register-class" @click="loginNavigation()">Ya estas registrado? Llevame a la pantalla de inicio de sesion.</p>
    <div class="modal-bottom"></div>
  </div>
</template>

<script>
import VueRecaptcha from 'vue-recaptcha';
import Config from '@/config.json';

export default {
  name: 'RegisterComponent',
  components: {
    VueRecaptcha
  },
  data() {
    return {
      username: '',
      password: '',
      confirmPassword: '',
      phone: '',
      name: '',

      recaptchaKey: '',
      captchaResp: ''
    };
  },

  created() {
    this.recaptchaKey = Config.RECAPTCHA;
  },

  methods: {
    async onVerify(response) {
      this.captchaResp = response;
    },
    onExpired() {
      // this.resetRecaptcha();
      this.captchaResp = '';
    },

    registerClicked() {
      if (
        this.usernameState
        && this.passwordState
        && this.confirmPasswordState
        && this.phoneState
        && this.nameState
        && this.captchaResp.length > 0
      ) {
        this.$emit('register', {
          email: this.username,
          password: this.password,
          phone: this.phone,
          name: this.name,
          recaptcha: this.captchaResp
        });
      } else{
        alert("Porfavor, recuerde llenar todos los campos y marcar el cuadrito 'im not a robot'")
      }
    },

    loginNavigation() {
      this.$emit('loginNav');
    },

    validEmail(email) {
      // eslint-disable-next-line
      const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    }
  },
  computed: {
    usernameState() {
      if (this.username.length === 0) return null;
      return this.validEmail(this.username);
    },

    passwordState() {
      if (this.password.length === 0) return null;
      return this.password.length > 6;
    },

    confirmPasswordState() {
      if (this.confirmPassword.length === 0) return null;
      return this.confirmPassword === this.password;
    },

    phoneState() {
      if (this.phone.length === 0) return null;
      return this.phone.length === 10;
    },

    nameState() {
      if (this.name.length === 0) return null;
      return this.name.length > 0;
    }
  }
};
</script>

<style lang="scss">
.register-button {
  width: 100%;
  background-image: linear-gradient(to right, #136a8a, #267871) !important;
}

.register-class {
  padding-top: 20px;
  cursor: pointer;
}
</style>
