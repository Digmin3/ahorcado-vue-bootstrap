<template>
  <div>
    <img alt="Vue logo" src="../assets/ahorcado.svg">
    <h1>Bienvenido al ahorcado</h1>
    <b-form @submit="onSubmit">
      <b-form-group label="Selecciona la dificultad" class="d-flex justify-content-center">
        <b-form-radio class="text-left" v-model="dificultad" name="dificultad" value="1" required>Fácil</b-form-radio>
        <b-form-radio class="text-left" v-model="dificultad" name="dificultad" value="2" required>Medio</b-form-radio>
        <b-form-radio class="text-left" v-model="dificultad" name="dificultad" value="3" required>Difícil</b-form-radio>
      </b-form-group>
      <b-button type="submit" variant="primary">Jugar</b-button>
    </b-form>
  </div>
</template>

<script>
  export default {
    name: "SelectorDificultad",
    data: function () {
      return {
        dificultad: "1"
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault();
        this.$emit('dificultad', this.dificultad)
      }
    }
  }
</script>

<style scoped>

</style>
