<template>
  <transition name="fade" mode="out-in">
    <div class="home-view" v-if="mostrarConteudo">
      <header class="header">
        <nav class="nav">
          <a href="#about" class="nav-link">Sobre</a>
          <a href="#geoparks" class="nav-link">Geoparques</a>
          <a href="#contact" class="nav-link">Contato</a>
        </nav>
      </header>
      <section id="about" class="about">
        <h2>Sobre o GeoQuest</h2>
        <p>
          O GeoQuest é uma plataforma que ajuda você a descobrir geoparques incríveis ao redor do
          mundo. Nosso objetivo é promover a conscientização e a apreciação dessas maravilhas
          naturais.
        </p>
      </section>

      <section class="hero">
        <h1 class="hero-title">GEOQUEST</h1>
        <p class="hero-subtitle">Descubra o geoparque que mais combina com você!</p>
        <button class="start-btn" @click="startQuiz" aria-label="Começar o questionário">
          Começar
        </button>
      </section>

      <section id="geoparks" class="geoparks">
        <h2>Geoparques Destacados</h2>
        <div class="geopark-cards">
          <div class="geopark-card" v-for="geopark in featuredGeoparks" :key="geopark.id">
            <img :src="geopark.image" :alt="geopark.name" class="geopark-image" />
            <h3>{{ geopark.name }}</h3>
            <p>{{ geopark.description }}</p>
          </div>
        </div>
      </section>

      <section id="contact" class="contact">
        <h2>Contato</h2>
        <p>Entre em contato conosco para mais informações sobre geoparques.</p>
        <form class="contact-form">
          <input type="text" placeholder="Nome" class="input" />
          <input type="email" placeholder="Email" class="input" />
          <textarea placeholder="Mensagem" class="input"></textarea>
          <button type="submit" class="submit-btn">Enviar</button>
        </form>
      </section>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      mostrarConteudo: true,
      featuredGeoparks: [
        {
          id: 1,
          name: 'Geoparque A',
          image: 'path/to/image1.jpg',
          description: 'Descrição do Geoparque A.'
        },
        {
          id: 2,
          name: 'Geoparque B',
          image: 'path/to/image2.jpg',
          description: 'Descrição do Geoparque B.'
        },
        {
          id: 3,
          name: 'Geoparque C',
          image: 'path/to/image3.jpg',
          description: 'Descrição do Geoparque C.'
        }
      ]
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
/* Reset */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* Estilos gerais */
body {
  font-family: 'Arial, sans-serif';
  color: #333;
  line-height: 1.6;
  background-color: #f0f0f0;
}

.home-view {
  width: 100%;
  overflow-x: hidden;
}

/* Estilos para o header */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.logo {
  width: 150px;
}

.nav {
  display: flex;
}

.nav-link {
  margin-left: 20px;
  color: #007bff;
  text-decoration: none;
}

.nav-link:hover {
  text-decoration: underline;
}

/* Estilos para a seção hero */
.hero {
  text-align: center;
  padding: 100px 20px;
  background: url('path/to/hero-image.jpg') no-repeat center center/cover;
  color: white;
}

.hero-title {
  font-size: 3em;
  margin-bottom: 20px;
}

.hero-subtitle {
  font-size: 1.5em;
  margin-bottom: 40px;
}

.start-btn {
  background-color: #007bff;
  color: #fff;
  padding: 15px 30px;
  font-size: 1.2em;
  border: none;
  cursor: pointer;
  transition:
    background-color 0.3s,
    transform 0.3s;
  border-radius: 5px;
}

.start-btn:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.start-btn:focus {
  outline: 2px solid #0056b3;
  outline-offset: 2px;
}

/* Estilos para a seção about */
.about {
  padding: 60px 20px;
  background: #fff;
  text-align: center;
}

.about h2 {
  font-size: 2.5em;
  margin-bottom: 20px;
}

.about p {
  font-size: 1.2em;
  max-width: 800px;
  margin: 0 auto;
  line-height: 1.8;
}

/* Estilos para a seção geoparks */
.geoparks {
  padding: 60px 20px;
  text-align: center;
}

.geoparks h2 {
  font-size: 2.5em;
  margin-bottom: 20px;
}

.geopark-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.geopark-card {
  background: white;
  margin: 10px;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  max-width: 300px;
  text-align: left;
}

.geopark-image {
  width: 100%;
  border-radius: 10px 10px 0 0;
}

.geopark-card h3 {
  font-size: 1.5em;
  margin-top: 15px;
}

.geopark-card p {
  font-size: 1em;
  color: #666;
}

/* Estilos para a seção contact */
.contact {
  padding: 60px 20px;
  background: #f9f9f9;
  text-align: center;
}

.contact h2 {
  font-size: 2.5em;
  margin-bottom: 20px;
}

.contact p {
  font-size: 1.2em;
  margin-bottom: 20px;
}

.contact-form {
  max-width: 600px;
  margin: 0 auto;
}

.input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.submit-btn {
  background-color: #007bff;
  color: #fff;
  padding: 15px 30px;
  font-size: 1.2em;
  border: none;
  cursor: pointer;
  transition:
    background-color 0.3s,
    transform 0.3s;
  border-radius: 5px;
}

.submit-btn:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.submit-btn:focus {
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
