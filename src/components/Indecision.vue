<!--Siempre tiene que tener un hijo - elemento html-->
<template>

  <!--    <img src="https://via.placeholder.com/250" alt="bg"> v-bind basicamente para que valla en el html como dom real-->
  <img v-if="img" v-bind:src="img" alt="bg">
  <div class="bg-dark"></div>

  <div class="indecision-container">
    <!--    Input enlazado a propiedad  reactivo-->
    <input
        v-model="question"
        type="text"
        placeholder="Hazme una pregunta">
    <p>Recuerda agregar a lfinal un signo de interrogacion (?)</p>

    <div v-if="isValidQuestion">
      <h2>{{ question }}</h2>
      <h1>{{ answer }}</h1>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {    //Objeto Reactivo
      question: null,
      answer: null,
      img: null,
      isValidQuestion: false
    }
  },
  methods: {
    async getAnswers() {
      this.answer = 'Pensando...'
      const {answer, image} = await fetch("https://yesno.wtf/api").then(r => r.json());
      this.answer = answer === 'yes' ? 'Si!' : 'No!'
      this.img = image
    }
  },
  //Objetos que estan pendientes
  watch: {
    //Que hacer cuando el question cambie
    question(value, oldValue) {

      this.isValidQuestion = false;
      if (!value.includes('?')) return;

      this.isValidQuestion = true;

      //TODO: Realizar peticion http
      this.getAnswers();
    }
  }
}
</script>

<!--Indica que solo se aplicara los estilos a este componente-->
<style scoped>

img, .bg-dark {
  height: 100vh;
  left: 0px;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  top: 0px;
  width: 100vw;
}

img {
  height: 100vh;
  left: 0;
  object-fit: cover;
  position: fixed;
  top: 0;
  width: 100vw;
}

.bg-dark {
  background-color: rgba(0, 0, 0, 0.4);
}

.indecision-container {
  position: relative;
  z-index: 99;
}

input {
  width: 250px;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
}

input:focus {
  outline: none;
}

p {
  color: white;
  font-size: 20px;
  margin-top: 0px;
}

h1, h2 {
  color: white;
}

h2 {
  margin-top: 150px;
}

</style>