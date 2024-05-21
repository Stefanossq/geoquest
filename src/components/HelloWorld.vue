<template>
  <div class="hero min-h-screen bg-base-200">
    <div class="hero-content text-center">
      <div class="max-w-md">
        <h1 class="text-5xl font-bold mb-5">GeoQuest</h1>
        <transition name="fade" mode="out-in">
          <p v-if="currentPhrase" :key="currentPhrase" class="text-lg font-semibold">
            {{ currentPhrase }}
          </p>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      phrases: [
        'Explore as maravilhas dos geoparques',
        'Descubra trilhas incríveis',
        'Conheça a geologia local',
        'Participe de atividades educacionais',
        'Aventuras ao ar livre',
        'Preserve a natureza e aprenda',
        'Planeje sua visita a um geoparque'
      ],
      currentPhraseIndex: 0
    }
  },
  computed: {
    currentPhrase() {
      return this.phrases[this.currentPhraseIndex]
    }
  },
  mounted() {
    this.startPhraseRotation()
  },
  methods: {
    startPhraseRotation() {
      setInterval(() => {
        this.currentPhraseIndex = (this.currentPhraseIndex + 1) % this.phrases.length
      }, 3000) // Troca de frase a cada 3 segundos
    }
  }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
