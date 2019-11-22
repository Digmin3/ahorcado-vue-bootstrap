<template>
  <b-container>
    <b-alert :show="finJuego" variant="info">Fin del juego</b-alert>

    <b-row>
      <b-col md="4">
        <h1>{{vistaPalabra}}</h1>
        <div v-if="letrasUsadas.length !== 0">
          <h3>Letras usadas</h3>
          <h1>{{vistaUsadas}}</h1>
        </div>
      </b-col>

      <b-col md="4">
        <b-form @submit="onSubmit" v-if="!finJuego">
          <b-form-group label="Introduce una letra" class="d-flex justify-content-center">
            <b-form-input
                ref="inputLetra"
                autocomplete="off"
                id="letraIntento"
                v-model="letraIntento"
                required
                :state="contenidoValido"
                placeholder="Letra"
                :formatter="format"
                :disabled="finJuego"
            ></b-form-input>
            <b-form-invalid-feedback id="input-live-feedback">
              Solo una letra
            </b-form-invalid-feedback>
          </b-form-group>
          <b-button type="submit" variant="primary" :disabled="finJuego">Probar</b-button>
        </b-form>
      </b-col>
      <b-col md="4">
        <p>Intentos restantes: {{intentos}}</p>
        <b-button variant="warning" @click="reiniciar">Reiniciar partida</b-button>
        <p></p>
        <b-button variant="danger" @click="volver">Volver al menú</b-button>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  const FACIL = "1";
  const MEDIO = "2";
  const DIFICIL = "3";

  const palabras = ["Salamanca", "Zaragoza", "Madrid", "España", "Barcelona", "Castilla", "Francia", "Estados Unidos"];

  export default {
    name: "Juego",
    props: {
      dificultad: {
        type: String,
        required: true,
        default: DIFICIL
      },
    },
    data: function () {
      return {
        palabra: "",
        intentos: 0,
        letrasOcultas: 0,
        letrasAcertadas: 0,
        letrasUsadas: [],
        letrasPalabra: [],
        letraIntento: ""
      }
    },
    created: function () {
      this.palabra = palabras[Math.floor(Math.random() * palabras.length)].toUpperCase();

      for (let i = 0; i < this.palabra.length; i++) {
        if (!this.letrasPalabra.includes(this.palabra[i]) && this.palabra[i] !== ' ') {
          this.letrasPalabra.push(this.palabra[i]);
        }
      }
      this.letrasPalabra.sort();

      if (this.dificultad === FACIL) {
        this.letrasOcultas = Math.floor(this.palabra.length * 0.3);
        this.intentos = Math.floor(this.palabra.length * 1.5);
      } else if (this.dificultad === MEDIO) {
        this.letrasOcultas = Math.floor(this.palabra.length * 0.5);
        this.intentos = Math.floor(this.palabra.length);
      } else if (this.dificultad === DIFICIL) {
        this.letrasOcultas = Math.floor(this.palabra.length * 0.6);
        this.intentos = Math.floor(this.palabra.length * 0.8);
      }


    },
    computed: {
      vistaPalabra: function () {
        let retorno = "";
        for (let i = 0; i < this.palabra.length; i++) {
          if (this.letrasUsadas.includes(this.palabra[i]) || this.palabra[i] === ' ') {
            retorno += this.palabra[i];
          } else {
            retorno += "-";
          }
        }
        return retorno;
      },
      vistaUsadas: function () {
        let retorno = "";
        for (let i = 0; i < this.letrasUsadas.length; i++) {
          retorno += this.letrasUsadas[i];
          retorno += " ";
        }
        return retorno.trim();
      },
      contenidoValido: function () {
        if (this.letraIntento.length === 0) {
          return null;
        } else if (this.letraIntento.length === 1) {
          return true;
        } else {
          return false;
        }
      },
      finJuego: function () {
        if (this.intentos <= 0 || this.letrasAcertadas === this.letrasPalabra.length) {
          return true;
        } else {
          return false;
        }
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault();
        this.$refs["inputLetra"].focus();

        this.letraIntento = this.letraIntento.toUpperCase();

        if (this.letraIntento.length === 0 || this.letraIntento.length > 1) {
          this.letraIntento = "";
          return;
        }
        if (this.letrasUsadas.includes(this.letraIntento)) {
          this.letraIntento = "";
          return;
        }
        if (this.letrasPalabra.includes(this.letraIntento)) {
          this.letrasUsadas.push(this.letraIntento);
          this.letrasAcertadas += 1;
          this.letraIntento = "";
        } else {
          this.letrasUsadas.push(this.letraIntento);
          this.intentos -= 1;
          this.letraIntento = "";
        }
      },
      format(value) {
        return value.toUpperCase().trim();
      },
      reiniciar() {
        this.letrasAcertadas = 0;
        this.letrasUsadas = [];

        if (this.dificultad === FACIL) {
          this.intentos = Math.floor(this.palabra.length * 1.5);
        } else if (this.dificultad === MEDIO) {
          this.intentos = Math.floor(this.palabra.length);
        } else if (this.dificultad === DIFICIL) {
          this.intentos = Math.floor(this.palabra.length * 0.8);
        }
      },
      volver() {
        this.$emit('volver');
      }
    }
  }
</script>

<style scoped>

</style>
