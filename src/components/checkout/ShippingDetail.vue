<template>
  <div class="align-left shipping">
    <div v-if="allAddresses.length > 0" class="addresses">
      <ul class="shipping-list">
        <li
          v-for="(add, aIndex) in allAddresses"
          v-bind:key="aIndex"
          v-bind:class="{'selected' : addressEqual(add)}"
          @click="chooseAddress(add)"
        >
          <b-row>
            <b-col>
              {{add.firstName}} {{add.lastName}}
              <br>
              {{add.addressLine1}} {{add.addressLine2}}
              <br>
              {{add.state}} {{add.country}} 
            </b-col>
            <b-col>
              <div class="align-right">
                <a @click="editClicked(add)">Editar</a> &nbsp;&nbsp;&nbsp;
                <a @click="deleteClicked(add)">Borrar</a>
              </div>
            </b-col>
          </b-row>
        </li>
      </ul>
    </div>
    <div v-else-if="allAddresses.length <= 0 && !isShowAddAddress">
      <div class="align-center">
        <strong><p>Porfavor agrega una direccion de entrega</p></strong>
      </div>
    </div>

    <!-- Show list of existing addresses here with choice to choose -->
    <a @click="showAddAddress()" v-if="isSessionActive">
      <font-awesome-icon icon="plus"/>&nbsp;&nbsp; Agregar una nueva direccion
    </a>


    <!-- Input form to add new address for the user -->
    <transition
      name="shipping-form-anim"
      enter-active-class="animated slideInLeft faster"
      leave-active-class="animated slideOutLeft faster"
    >
      <div v-if="isShowAddAddress" class="shipping-form">
        <hr>
        <b-row>
          <b-col md="6">
            <b-form-group>
              <label for="firstName">Primer nombre:</label>
              <b-form-input
                id="firstName"
                type="text"
                name="firstName"

                
                size="sm"
                :state="firstNameState"
                v-model="shippingDeet.firstName"
                placeholder="Ingresa tu nombre"
                aria-describedby="firstNameFeedback"
              ></b-form-input>
              <b-form-invalid-feedback id>
                <!-- This will only be shown if the preceeding input has an invalid state -->
                Este campo no puede quedar vacio.
              </b-form-invalid-feedback>
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group>
              <label for="lastName">Apellido:</label>
              <b-form-input
                id="lastName"
                size="sm"
                type="text"
                name="lastName"
                v-model="shippingDeet.lastName"
                placeholder="Ingresa tu apellido"
              ></b-form-input>
            </b-form-group>
          </b-col>
        </b-row>

        <b-form-group>
          <label for="address1">Direccion principal:</label>
          <b-form-input
            id="address1"
            type="text"
            size="sm"
            name="text"
            :state="address1State"
            v-model="shippingDeet.addressLine1"
            placeholder="Ingresa tu direccion"
            aria-describedby="address1State"
          ></b-form-input>
          <b-form-invalid-feedback id="address1State">
            <!-- This will only be shown if the preceeding input has an invalid state -->
            Porfavor ingresa tu direccion antes de continuar.
          </b-form-invalid-feedback>
        </b-form-group>

        <b-form-group>
          <label for="address2">Instrucciones adicionales de entrega:</label>
          <b-form-input
            id="address2"
            type="text"
            name="address2"
            size="sm"
            v-model="shippingDeet.addressLine2"
            placeholder="Rango de horas preferidas para recibir el pedido, casa, conjunto, bloque, etc..."
          ></b-form-input>
        </b-form-group>

        <b-row>
          <b-col>
            <b-form-group>
              <label for="city">Ciudad:</label>
              <b-form-select
                id="city"
                type="text"
                name="text"
                :options="cityOptions"                

                :state="cityState"
                size="sm"
                v-model="shippingDeet.city"
                placeholder="Ingresa tu ciudad"
                aria-describedby="cityStatee"
              ></b-form-select>
              <b-form-invalid-feedback id="cityStatee">
                <!-- This will only be shown if the preceeding input has an invalid state -->
                Porfavor ingresa la ciudad antes de continuar.
              </b-form-invalid-feedback>
            </b-form-group>
          </b-col>
          <b-col>
            <b-form-group>
              <label for="phone">Celular:</label>
              <b-form-input
                id="phone"
                type="number"
                name="text"
                :state="phoneState"
                size="sm"
                v-model="shippingDeet.mobilePhone"
                placeholder="Ingresa tu celular"
                aria-describedby="phoneStatee"
              ></b-form-input>
              <b-form-invalid-feedback id="phoneStatee">
                <!-- This will only be shown if the preceeding input has an invalid state -->
                porfavor ingresa un celular antes de continuar.
              </b-form-invalid-feedback>
            </b-form-group>
          </b-col>
        </b-row>

        <b-row>
          <b-col>
            <b-form-group>
              <label for="state">Localidad:</label>
              <b-form-select
                id="state"
                type="text"
                name="state"
                size="sm"
                :options="localidadOptions"                  
                :state="stateState"
                v-model="shippingDeet.state"
                placeholder="Ingresa tu localidad"
                aria-describedby="stateFeedback"
              ></b-form-select>
              <b-form-invalid-feedback id="stateFeedback">
                <!-- This will only be shown if the preceeding input has an invalid state -->
                POrfavor ingresa la localidad.
              </b-form-invalid-feedback>
            </b-form-group>
          </b-col>
          <b-col>
            <b-form-group>
              <label for="zip">Codigo ZIP:</label>
              <b-form-input
                id="zip"
                type="number"
                name="zip"
                size="sm"
                :state="zipState"
                v-model="shippingDeet.zipCode"
                placeholder="Si no lo sabes, ingresa 111111."
                aria-describedby="zipFeedback"
              ></b-form-input>
              <b-form-invalid-feedback id="zipFeedback">
                <!-- This will only be shown if the preceeding input has an invalid state -->
                Codigo ZIP no puede quedar vacio, si no lo sabes ingresa 111111.
              </b-form-invalid-feedback>
            </b-form-group>
          </b-col>
          <b-col>
            <b-form-group>
              <label for="country">Pais</label>
              <b-form-select
                v-model="shippingDeet.country"
                :options="countryOptions"
                size="sm"
                :state="countryState"
                name="country"
                id="country"
              />
              <b-form-invalid-feedback id="countryFeedback">
                <!-- This will only be shown if the preceeding input has an invalid state -->
                El pais no puede quedar vacio.
              </b-form-invalid-feedback>
            </b-form-group>
          </b-col>
        </b-row>
        <div class="action-buttons">
          <b-button variant="secondary-button" class="cancel-btn" @click="cancelForm()">Cancelar</b-button>
          <b-button class="primary-button" v-if="!isUpdate" @click="saveAddress()">Guardar</b-button>
          <b-button class="primary-button" v-else @click="saveAddress()">Editar</b-button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import ShippingDTO from '@/dto/ShippingAddress.json';
import { mapGetters } from 'vuex';

export default {
  name: 'ShippingDetail',
  async created() {
    this.shippingDeet = ShippingDTO;
    await this.$store.dispatch('shippingStore/addressAction', {
      address: null,
      action: 'get',
    });

    if (this.allAddresses.length > 0 && !this.selectedAddress) {
      this.$emit('selected', this.allAddresses[0]);
    }
  },
  data() {
    return {
      isShowAddAddress: false,
      shippingDeet: null,
      description: '',
      isUpdate: false,
      countryOptions: ['Colombia'],
      cityOptions: ['Bogota'],
      localidadOptions: ['Suba', 'Usaquen', 'Engativa', 'Chapinero', 'Santa Fe', 'San Cristobal', 'Usme', 'Tunjuelito', 'Bosa', 'Kennedy', 'Fontibon', 'Barrios Unidos', 'Teusaquillo', 'Los Martires', 'Antonio Nariño', 'Puente Aranda', 'Candelaria', 'Rafael Uribe', 'Ciudad Bolivar']
    };
  },

  methods: {
    showAddAddress() {
      this.isShowAddAddress = true;
    },
    hideAddAddress() {
      this.isShowAddAddress = false;
    },

    cancelForm() {
      this.resetFields();
      this.isShowAddAddress = false;
      this.isUpdate = false;
    },

    resetFields() {
      // eslint-disable-next-line
      for (const key in this.shippingDeet) {
        this.shippingDeet[key] = null;
      }
    },

    addressEqual(givenAdd) {
      return _.isEqual(givenAdd, this.selectedAddress);
    },

    chooseAddress(add) {
      this.$emit('selected', add);
    },

    editClicked(address) {
      this.isUpdate = true;
      this.shippingDeet = _.cloneDeep(address);
      this.isShowAddAddress = true;
    },

    deleteClicked(address) {
      const cloned = _.cloneDeep(address);
      this.$store.dispatch('shippingStore/addressAction', {
        address: cloned,
        action: 'delete',
      });
      this.$router.go()
    },

    async saveAddress() {
      // eslint-disable-next-line no-restricted-syntax
      for (const key in this.shippingDeet) {
        if (this.shippingDeet[key] == null) {
          this.shippingDeet[key] = '';
        }
      }
      if (
        this.firstNameState
        && this.address1State
        && this.stateState
        && this.zipState
        && this.countryState
        && this.cityState
      ) {
        const cloned = _.cloneDeep(this.shippingDeet);
        const res = await this.$store.dispatch('shippingStore/addressAction', {
          address: cloned,
          action: this.isUpdate ? 'put' : 'post',
        });

        if (res) {
          this.isUpdate = false;
          // this.allAddresses.push(cloned);
          // this.$emit('selected', cloned);

          this.resetFields();
          this.isShowAddAddress = false;
        }
        this.$router.go()
      }
    },
  },

  computed: {
    ...mapGetters({
      allAddresses: 'shippingStore/allAddresses',
      selectedAddress: 'shippingStore/getSelectedAddress',
      isSessionActive: 'authStore/isSessionActive',
    }),

    cityState() {
      if (this.shippingDeet.city == null) return null;
      return this.shippingDeet.city.length >= 1;
    },

    phoneState() {
      if (this.shippingDeet.mobilePhone == null) return null;
      return this.shippingDeet.mobilePhone.length >= 1;
    },

    firstNameState() {
      if (this.shippingDeet.firstName == null) return null;
      return this.shippingDeet.firstName.length >= 1;
    },

    address1State() {
      if (this.shippingDeet.addressLine1 == null) return null;
      return this.shippingDeet.addressLine1.length > 0;
    },

    stateState() {
      if (this.shippingDeet.state == null) return null;
      return this.shippingDeet.state.length > 0;
    },

    zipState() {
      if (this.shippingDeet.zipCode == null) return null;
      return this.shippingDeet.zipCode.length > 0;
    },

    countryState() {
      if (this.shippingDeet.country == null) return null;
      return this.shippingDeet.country.length > 0;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../../assets/css/global.scss';
.shipping-form {
  padding: 10px 0px;
  // background: white;

  .cancel-btn {
    margin-right: 10px;
    margin-top: 2em;
  }

  .action-buttons {
    padding: 10px 0px;
  }
}

.shipping {
  margin-top: 30px;
}

.shipping-list {
  list-style-type: none;
  padding: 0px;
  margin-bottom: 20px;
  li {
    padding: 10px;
    padding-left: 1.2em;
    margin-bottom: 10px;

    &.selected {
      // background: $gradient-color;
      background: $pitch-black;
      color: white;
    }

    &:hover {
      cursor: pointer;
    }
  }
}
</style>
