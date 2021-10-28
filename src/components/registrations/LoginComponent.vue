<template>
  <div>
    <div class="header">
      <h2>
        <p v-if="!forgotEnabled">Inicio de sesion</p>
        <p v-else>Olvidaste tu contraseña?</p>
      </h2>
      <p v-if="forgotEnabled">Porfavor ingresa tu email para reiniciar tu contraseña</p>
    </div>

    <b-form-group>
      <b-form-input
        id="username"
        type="email"
        name="username"
        :state="usernameState"
        v-model="username"
        placeholder="Ingresa tu correo"
        aria-describedby="usernameFeedback"
      ></b-form-input>
      <b-form-invalid-feedback id="usernameFeedback" class="align-left">
        <!-- This will only be shown if the preceeding input has an invalid state -->
        Porfavor ingresa un correo valido
      </b-form-invalid-feedback>
    </b-form-group>

    <b-form-group>
      <b-form-input
        v-if="!forgotEnabled"
        type="password"
        name="password"
        v-model="password"
        @keyup.native.enter="loginClicked()"
        placeholder="Ingresa tu contraseña"
        aria-describedby="passwordFeedback"
      ></b-form-input>
      <!-- <b-form-invalid-feedback id="passwordFeedback">Enter at least 6 characters.</b-form-invalid-feedback> -->
    </b-form-group>
    <!-- <vue-recaptcha @verify="onVerify" @expired="onExpired" :sitekey="recaptchaKey"></vue-recaptcha> -->
    <!-- <p class="info align-left">Porfavor resuelve el captcha antes de iniciar sesion.</p> -->

    <p class="forget-password" v-if="!forgotEnabled" @click="forgetPassword()">Olvidaste tu contraseña?</p>

    <b-btn class="login-button" v-if="!forgotEnabled" @click="loginClicked()">Iniciar sesion</b-btn>
    <b-btn class="login-button" v-if="forgotEnabled" @click="resetPassword()">Reestablecer la contraseña</b-btn>

    <strong><p class="register-class" @click="register()">Nuevo usuario? Registrate aqui.</p></strong>
    <div class="modal-bottom"></div>
  </div>
</template>

<script>
// import VueRecaptcha from 'vue-recaptcha';
import ProxyUrl from '@/constants/ProxyUrls';
import Config from '@/config.json';

export default {
  name: 'LoginComponent',
  components: {
    // VueRecaptcha
  },  
  data() {
    return {
      username: '',
      password: '',
      forgotEnabled: false,
      recaptchaKey: '',
      captchaResp: '',
    };
  },

  created() {
    this.recaptchaKey = Config.RECAPTCHA;
  },

  methods: {
    // async onVerify(response) {
    //   this.captchaResp = response;
    // },
    // onExpired() {
    //   // this.resetRecaptcha();
    //   this.captchaResp = '';
    // },

    validEmail(email) {
      // eslint-disable-next-line
      const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },

    forgetPassword() {
      this.forgotEnabled = true;
      this.password = '';
    },

    // async captchaValidate() {
    //   try {
    //     let {data} = await this.$axios({
    //       headers: {
    //         'Access-Control-Allow-Origin': '*',
    //       },
    //       url: ProxyUrl.recaptcha,
    //       method: 'post',
    //       params: {
    //         secret: Config.RECAPTCHA_SECRET,
    //         response: this.captchaResp,
    //       }
    //     });

    //     if(data && data.success){
    //       return true;
    //     }else return false;
    //   } catch (error) {
    //     return false;
    //   }

    // },

    async loginClicked() {
      // let isValidated = await this.captchaValidate();
      // if (this.usernameState && this.captchaResp.length > 0) {
      if (this.usernameState > 0) {
        this.$emit('login', {
          email: this.username,
          password: this.password,
          recaptcha: this.captchaResp,
        });
      } else {
        alert("Porfavor intente nuevamente")
      }
    },

    async resetPassword() {
      if (this.usernameState) {
        try {
          const { data } = await this.$axios({
            method: 'get',
            url: ProxyUrl.forgotPassword + this.username,
          });

          if (data && data.httpStatus === 200) {
            this.$emit('close');

            this.$notify({
              group: 'all',
              type: 'success',
              text: 'El correo fue enviado satisfactoriamente. Porfavor revisa tu correo y sigue las instrucciones.',
            });
          }
        } catch (err) {
          this.$notify({
            group: 'all',
            type: 'error',
            text: 'El correo no pudo ser enviado.Porfavor intenta nuevamente',
          });
        }
      }
    },

    register() {
      this.$emit('register');
    },
  },
  computed: {
    usernameState() {
      if (this.username.length === 0) return null;
      return this.validEmail(this.username);
    },
  },
};
</script>

<style lang='scss'>
.forget-password {
  text-align: right;
  cursor: pointer;
}

.header {
  // margin: 50px 0px;
  color: #267871;

  h2 {
    margin-bottom: 1em;
  }
}
.login-button {
  width: 100%;
  background-image: linear-gradient(to right, #267871, #136a8a) !important;
}

.register-class {
  padding-top: 20px;
  cursor: pointer;
}
</style>
