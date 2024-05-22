<template>
  <transition name="fade" mode="out-in">
    <div class="home-view" v-if="mostrarConteudo">
      <header class="header">
        <nav class="navbar bg-base-100 shadow-lg">
          <div class="flex-1">
            <a class="btn btn-ghost normal-case text-xl" href="#">GeoQuest</a>
          </div>
          <div class="flex-none">
            <ul class="menu menu-horizontal px-1">
              <li><a href="#about">Sobre</a></li>
              <li><a href="#geoparks">Geoparques</a></li>
              <li><a href="#contact">Contato</a></li>
            </ul>
          </div>
        </nav>
      </header>

      <section id="about" class="about text-center p-10 bg-base-200">
        <h2 class="text-3xl font-bold mb-5">Sobre o GeoQuest</h2>
        <p class="text-lg max-w-2xl mx-auto">
          O GeoQuest é uma plataforma que ajuda você a descobrir geoparques incríveis ao redor do
          mundo. Nosso objetivo é promover a conscientização e a apreciação dessas maravilhas
          naturais.
        </p>
      </section>

      <section
        class="hero text-center p-20 bg-cover bg-center text-white"
        style="background-image: url('path/to/hero-image.jpg')"
      >
        <h1 class="text-5xl font-bold mb-5">GEOQUEST</h1>
        <p class="text-2xl mb-10">Descubra o geoparque que mais combina com você!</p>
        <button class="btn btn-primary btn-lg" @click="startQuiz">Começar</button>
      </section>

      <section id="geoparks" class="geoparks text-center p-10 bg-base-200">
        <h2 class="text-3xl font-bold mb-5">Geoparques Destacados</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5">
          <div
            class="card bg-base-100 shadow-xl transition-transform duration-300 hover:scale-105 p-5 rounded-lg"
            v-for="geopark in featuredGeoparks"
            :key="geopark.id"
          >
            <figure>
              <img :src="geopark.image" :alt="geopark.name" class="geopark-image rounded-t-lg" />
            </figure>
            <div class="card-body">
              <h3 class="card-title">{{ geopark.name }}</h3>
              <p>{{ geopark.description }}</p>
            </div>
          </div>
        </div>
      </section>

      <section id="contact" class="contact text-center p-10 bg-base-200">
        <h2 class="text-3xl font-bold mb-5">Contato</h2>
        <p class="text-lg mb-5">Entre em contato conosco para mais informações sobre geoparques.</p>
        <form class="max-w-lg mx-auto">
          <input
            type="text"
            placeholder="Nome"
            class="input input-bordered w-full mb-5"
            aria-label="Nome"
          />
          <input
            type="email"
            placeholder="Email"
            class="input input-bordered w-full mb-5"
            aria-label="Email"
          />
          <textarea
            placeholder="Mensagem"
            class="textarea textarea-bordered w-full mb-5"
            aria-label="Mensagem"
          ></textarea>
          <button type="submit" class="btn btn-primary btn-lg w-full">Enviar</button>
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
      }, 500)
    }
  }
}
</script>

<style scoped>
/* Estilos gerais */
body {
  font-family: 'Arial, sans-serif';
  background-color: #f8f9fa;
}

.home-view {
  width: 100%;
  overflow-x: hidden;
}

/* Estilos para a transição */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

/* Estilos para os botões */
.btn-primary {
  transition:
    background-color 0.3s,
    transform 0.3s;
}

.btn-primary:hover {
  transform: scale(1.05);
}

/* Estilos para o card de geoparques */
.geopark-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-body {
  padding: 1rem;
}

.card-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.card {
  border-radius: 0.5rem;
}
</style>
