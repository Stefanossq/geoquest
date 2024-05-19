<template>
  <div class="p-4 max-w-lg mx-auto">
    <!-- Exibição do questionário -->
    <div v-if="indicePerguntaAtual < perguntas.length" class="card shadow-lg p-6">
      <h2 class="text-2xl font-bold mb-4">{{ perguntas[indicePerguntaAtual].titulo }}</h2>
      <div v-if="perguntas[indicePerguntaAtual].tipo === 'numero'">
        <input
          type="number"
          v-model="respostas[indicePerguntaAtual]"
          :min="perguntas[indicePerguntaAtual].min"
          :max="perguntas[indicePerguntaAtual].max"
          @input="atualizarResposta($event.target.value)"
          @blur="validateField"
          :class="[
            'input input-bordered w-full mb-2',
            { 'input-error': !respostas[indicePerguntaAtual] && attemptedSubmit }
          ]"
        />
        <span v-if="!respostas[indicePerguntaAtual] && attemptedSubmit" class="text-error text-sm"
          >Este campo é obrigatório.</span
        >
      </div>
      <div v-else-if="perguntas[indicePerguntaAtual].tipo === 'select'">
        <select
          v-model="respostas[indicePerguntaAtual]"
          @blur="validateField"
          :class="[
            'select select-bordered w-full mb-2',
            { 'input-error': !respostas[indicePerguntaAtual] && attemptedSubmit }
          ]"
        >
          <option disabled value="">{{ perguntas[indicePerguntaAtual].placeholder }}</option>
          <option v-for="opcao in perguntas[indicePerguntaAtual].opcoes" :key="opcao">
            {{ opcao }}
          </option>
        </select>
        <span v-if="!respostas[indicePerguntaAtual] && attemptedSubmit" class="text-error text-sm"
          >Este campo é obrigatório.</span
        >
      </div>
      <div class="flex justify-between">
        <button
          class="btn btn-secondary mt-4"
          @click="voltarPergunta"
          v-if="indicePerguntaAtual > 0"
        >
          Voltar
        </button>
        <button class="btn btn-primary mt-4" @click="proximaPergunta">Próxima</button>
      </div>
      <progress
        class="progress progress-primary w-full mt-4"
        :value="indicePerguntaAtual + 1"
        :max="perguntas.length"
      ></progress>
      <div class="text-center mt-4 text-sm">
        Pergunta {{ indicePerguntaAtual + 1 }} de {{ perguntas.length }}
      </div>
    </div>

    <!-- Exibição do resultado -->
    <div v-else class="card shadow-lg p-6 text-center">
      <h2 class="text-2xl font-bold mb-4">Geoparque Recomendado: {{ geoparqueRecomendado }}</h2>
      <p v-if="geoparqueRecomendadoInfo">{{ geoparqueRecomendadoInfo }}</p>
      <button class="btn btn-secondary w-full mt-4" @click="reiniciarQuestionario">
        Reiniciar
      </button>
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
      geoparqueRecomendadoInfo: null,
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
    voltarPergunta() {
      if (this.indicePerguntaAtual > 0) {
        this.indicePerguntaAtual--
      }
    },
    validarCampo() {
      this.attemptedSubmit = !this.respostas[this.indicePerguntaAtual]
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
          this.geoparqueRecomendadoInfo = respostaPredefinida.info // Supondo que exista uma descrição
          return
        }
      }
      this.geoparqueRecomendado = 'Nenhum geoparque encontrado para as suas respostas.'
      this.geoparqueRecomendadoInfo = null
    },
    reiniciarQuestionario() {
      this.indicePerguntaAtual = 0
      this.respostas = Array(this.perguntas.length).fill('')
      this.geoparqueRecomendado = null
      this.geoparqueRecomendadoInfo = null
      this.attemptedSubmit = false
    }
  }
}
</script>

<style scoped>
.input-error {
  border-color: #e3342f; /* Usando a cor de erro padrão do DaisyUI */
}
.text-error {
  color: #e3342f;
}
</style>
