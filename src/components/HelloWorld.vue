<template>
  <div class="hero min-h-screen bg-gradient-to-r from-cyan-400 to-teal-500">
    <div class="hero-content text-center">
      <div class="max-w-md">
        <h1 class="text-5xl font-bold mb-5">GeoQuest</h1>
        <div class="typed-text">
          <p v-if="currentPhrase" :key="currentPhrase" class="text-lg font-semibold">
            {{ currentPhrase }}
          </p>
        </div>
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
      currentPhraseIndex: 0,
      currentPhrase: '',
      letterIndex: 0,
      typingSpeed: 70, // Velocidade de digitação (em milissegundos)
      cursorVisible: true
    }
  },
  mounted() {
    this.typePhrase()
    setInterval(this.toggleCursor, 500) // Alternar a visibilidade do cursor a cada 500ms
  },
  methods: {
    typePhrase() {
      const phrase = this.phrases[this.currentPhraseIndex]
      if (this.letterIndex < phrase.length) {
        this.currentPhrase += phrase.charAt(this.letterIndex)
        this.letterIndex++
        setTimeout(this.typePhrase, this.typingSpeed)
      } else {
        // Reset para a próxima frase
        setTimeout(this.resetPhrase, 2000) // Tempo de espera antes de começar a próxima frase
      }
    },
    resetPhrase() {
      this.currentPhrase = ''
      this.letterIndex = 0
      this.currentPhraseIndex = (this.currentPhraseIndex + 1) % this.phrases.length
      this.typePhrase()
    },
    toggleCursor() {
      this.cursorVisible = !this.cursorVisible
    }
  }
}
</script>

<style scoped>
.hero {
  overflow: hidden; /* Para ocultar as letras digitadas fora da tela */
}

.typed-text p {
  overflow: hidden;
  white-space: nowrap;
  display: inline-block;
}

.typed-cursor {
  display: inline-block;
  width: 0.6em; /* Largura do cursor */
  height: 1em; /* Altura do cursor */
  background-color: black; /* Cor do cursor */
  animation: blink 1s infinite; /* Animação de piscar */
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
