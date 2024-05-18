<template>
  <transition name="fade" mode="out-in">
    <div class="home-view" v-if="mostrarConteudo">
      <h1>GEOQUEST: Aqui você coloca informações relevantes do app</h1>
      <p>Contexto e desenvolvimento sobre o assunto geoparque.</p>
      <button @click="startQuiz" aria-label="Começar o questionário">Começar</button>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      mostrarConteudo: true
    }
  },
  methods: {
    startQuiz() {
      this.mostrarConteudo = false
      setTimeout(() => {
        this.$router.push({ name: 'QuestionarioView' })
        console.log('Questionário iniciado!')
      }, 500) // Tempo para a transição ocorrer
    }
  }
}
</script>

<style scoped>
/* Estilos específicos para o componente HomeView */
.home-view {
  text-align: center;
  margin: 50px auto;
  max-width: 600px;
  padding: 0 20px;
}

.home-view h1 {
  font-size: 2.5em;
  color: #333;
  margin-bottom: 20px;
}

.home-view p {
  font-size: 1.2em;
  color: #666;
  margin-bottom: 40px;
}

.home-view button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  font-size: 1.1em;
  border: none;
  cursor: pointer;
  transition:
    background-color 0.3s,
    transform 0.3s;
  border-radius: 5px;
}

.home-view button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.home-view button:focus {
  outline: 2px solid #0056b3;
  outline-offset: 2px;
}

/* Estilos para a transição */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
</style>
