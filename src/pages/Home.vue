<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-md-4 offset-md-4">
          <div class="">
            <div class="form-group">
              <label>Nº Tarjeta Bip</label>
              <input type="text" class="form-control form-control-lg" v-model="numberCard" minlength="1" maxlength="8">
              <small v-if="!numberCard">Ingresa el número.</small>
            </div>
            <button type="button"
              class="btn btn-primary"
              v-on:click="consultarSaldo"
              v-bind:disabled="!isValidButton()">
              Consultar saldo
            </button>
          </div>
          <div v-if="loading" :class="$style.contentLoading">
            <div :class="$style.ldsdualring"></div>
            <p>Espere un momento</p>
          </div>
          <div :class="$style.CardDetails" v-if="SaldoDetalle">
            <ul class="list-group">
              <li class="list-group-item d-flex justify-content-between align-items-center">
                ID
                <span class="badge badge-default badge-pill">{{ items.id }}</span>
              </li>
              <li class="list-group-item d-flex justify-content-between align-items-center">
                Estado
                <span class="badge badge-default badge-pill">{{ items.estadoContrato }}</span>
              </li>
              <li class="list-group-item d-flex justify-content-between align-items-center">
                Fecha
                <span class="badge badge-default badge-pill">{{ items.fechaSaldo }}</span>
              </li>
              <li class="list-group-item d-flex justify-content-between align-items-center">
                Saldo disponible
                <span class="badge badge-success badge-pill" style="font-size: 18px;">{{ items.saldoTarjeta }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      numberCard: [],
      SaldoDetalle: false,
      loading: false,
      items: ""
    };
  },
  methods: {
    consultarSaldo() {
      this.$data.loading = true;
      var Api_Bip =
        "http://bip-servicio.herokuapp.com/api/v1/solicitudes.json?bip=";
      fetch(Api_Bip + this.numberCard)
        .then(response => response.json())
        .then(data => {
          this.items = data;
          this.$data.SaldoDetalle = true;
          this.$data.loading = false;
        });
      this.$data.SaldoDetalle = false;
    },

    isValidButton() {
      return this.$data.numberCard != "";
    }
  }
};
</script>

<style lang="scss" module>
body {
  background-color: #fafafa;
  margin: 8em auto;
}

.CardDetails {
  margin-top: 20px;
  & span {
    font-size: 16px;
    font-weight: 400;
  }
}

.contentLoading {
  text-align: center;
}

.ldsdualring {
  display: block;
  width: 64px;
  margin: auto auto;
  margin-top: 3em;
  margin-bottom: 0;
  height: 64px;
}

.ldsdualring:after {
  content: " ";
  display: block;
  width: 46px;
  height: 46px;
  margin: 1px;
  border-radius: 50%;
  border: 5px solid #757575;
  border-color: #757575 transparent #757575 transparent;
  animation: ldsdualringLoading 1.2s linear infinite;
}
@keyframes ldsdualringLoading {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>