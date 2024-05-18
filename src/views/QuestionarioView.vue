<template>
  <div>
    <!-- Exibição do questionário -->
    <div v-if="indicePerguntaAtual < perguntas.length">
      <h2>{{ perguntas[indicePerguntaAtual].titulo }}</h2>
      <div v-if="perguntas[indicePerguntaAtual].tipo === 'numero'">
        <input
          type="number"
          v-model="respostas[indicePerguntaAtual]"
          :min="perguntas[indicePerguntaAtual].min"
          :max="perguntas[indicePerguntaAtual].max"
          @input="atualizarResposta($event.target.value)"
          :class="{ 'input-error': !respostas[indicePerguntaAtual] && attemptedSubmit }"
        />
        <span v-if="!respostas[indicePerguntaAtual] && attemptedSubmit" class="error-msg"
          >Este campo é obrigatório.</span
        >
      </div>
      <div v-else-if="perguntas[indicePerguntaAtual].tipo === 'select'">
        <select
          v-model="respostas[indicePerguntaAtual]"
          :class="{ 'input-error': !respostas[indicePerguntaAtual] && attemptedSubmit }"
        >
          <option disabled value="">{{ perguntas[indicePerguntaAtual].placeholder }}</option>
          <option v-for="opcao in perguntas[indicePerguntaAtual].opcoes" :key="opcao">
            {{ opcao }}
          </option>
        </select>
        <span v-if="!respostas[indicePerguntaAtual] && attemptedSubmit" class="error-msg"
          >Este campo é obrigatório.</span
        >
      </div>
      <button @click="proximaPergunta">Próxima</button>
      <div class="progress">Pergunta {{ indicePerguntaAtual + 1 }} de {{ perguntas.length }}</div>
    </div>

    <!-- Exibição do resultado -->
    <div v-else>
      <h2>Geoparque Recomendado: {{ geoparqueRecomendado }}</h2>
      <button @click="reiniciarQuestionario">Reiniciar</button>
    </div>
  </div>
</template>

<script>
import respostasJSON from './respostas.json'

export default {
  data() {
    return {
      perguntas: [
        { titulo: 'Qual é a sua idade?', tipo: 'numero', min: 6, max: 99 },
        {
          titulo: 'Qual é o seu gênero?',
          tipo: 'select',
          opcoes: ['Masculino', 'Feminino', 'Outro'],
          placeholder: 'Selecione'
        },
        {
          titulo: 'Você já visitou algum geoparque antes?',
          tipo: 'select',
          opcoes: ['Sim', 'Não'],
          placeholder: 'Selecione'
        },
        {
          titulo: 'Escolha uma das imagens abaixo:',
          tipo: 'select',
          opcoes: ['Imagem 1', 'Imagem 2', 'Imagem 3', 'Imagem 4'],
          placeholder: 'Selecione'
        },
        {
          titulo: 'Qual é o seu clima favorito?',
          tipo: 'select',
          opcoes: ['Quente', 'Frio', 'Temperado', 'Tropical', 'Árido'],
          placeholder: 'Selecione'
        },
        {
          titulo: 'Qual é o seu tipo de paisagem favorito?',
          tipo: 'select',
          opcoes: ['Montanhas', 'Praias', 'Planícies', 'Florestas', 'Desertos'],
          placeholder: 'Selecione'
        },
        {
          titulo: 'O que você mais gosta de fazer ao ar livre?',
          tipo: 'select',
          opcoes: ['Caminhadas', 'Surfar', 'Observar aves', 'Acampar', 'Explorar cavernas'],
          placeholder: 'Selecione'
        },
        {
          titulo: 'Você gosta de praticar esportes aquáticos?',
          tipo: 'select',
          opcoes: ['Sim', 'Não'],
          placeholder: 'Selecione'
        }
      ],
      respostas: Array(8).fill(''), // Preenche com strings vazias inicialmente
      indicePerguntaAtual: 0,
      geoparqueRecomendado: null,
      respostasPredefinidas: [],
      attemptedSubmit: false
    }
  },
  mounted() {
    this.respostasPredefinidas = respostasJSON.respostas_predefinidas
  },
  methods: {
    atualizarResposta(valor) {
      this.$set(this.respostas, this.indicePerguntaAtual, String(valor).trim())
    },
    proximaPergunta() {
      this.attemptedSubmit = true
      if (this.respostas[this.indicePerguntaAtual]) {
        this.attemptedSubmit = false
        this.indicePerguntaAtual++
        if (this.indicePerguntaAtual === this.perguntas.length) {
          this.avaliarResultado()
        }
      }
    },
    avaliarResultado() {
      const respostasUsuario = this.respostas.map((resposta) => String(resposta).trim())
      for (const respostaPredefinida of this.respostasPredefinidas) {
        const respostasPredef = respostaPredefinida.respostas.map((resposta) =>
          String(resposta).trim()
        )
        let match = true
        for (let i = 0; i < respostasPredef.length; i++) {
          if (respostasUsuario[i] !== respostasPredef[i]) {
            match = false
            break
          }
        }
        if (match) {
          this.geoparqueRecomendado = respostaPredefinida.geoparque_recomendado
          return
        }
      }
      this.geoparqueRecomendado = 'Nenhum geoparque encontrado para as suas respostas.'
    },
    reiniciarQuestionario() {
      this.indicePerguntaAtual = 0
      this.respostas = Array(this.perguntas.length).fill('')
      this.geoparqueRecomendado = null
      this.attemptedSubmit = false
    }
  }
}
</script>

<style scoped>
.input-error {
  border: 1px solid red;
}
.error-msg {
  color: red;
  font-size: 12px;
}
.progress {
  margin-top: 20px;
}
</style>
