<template>
  <v-dialog v-model="modalVisible">
    <v-btn slot="activator">Utwórz nowe pytanie</v-btn>
    <v-card>
      <v-card-text>

        <p>Do tej pory quiz ma {{ numberOfQuestions }} pytań.</p>
        <v-text-field v-model="question" label="Pytanie"></v-text-field>
        <v-text-field v-model="answer1" label="Odpowiedź 1"></v-text-field>
        <v-text-field v-model="answer2" label="Odpowiedź 2"></v-text-field>
        <v-text-field v-model="correctAnswerIndex" label="Indeks prawidłowej odpowiedzi"></v-text-field>
        <v-btn v-on:click="add">dodaj pytanie</v-btn>
        <v-btn v-on:click="closeModal">zamknij</v-btn>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>
<script>
  export default {
    name: 'add-question',
    data () {
      return {
        question: '',
        answer1: '',
        answer2: '',
        correctAnswerIndex: null,
        modalVisible: false
      }
    },
    props: {
      numberOfQuestions: {type: Number}
    },
    computed: {
      normalizedQuestion () {
        return {
          title: this.question,
          answers: [this.answer1, this.answer2],
          correctAnswerIndex: +this.correctAnswerIndex
        }
      }
    },
    methods: {
      add () {
        this.$emit('new-question-added', this.normalizedQuestion)
      },
      closeModal () {
        this.modalVisible = false
      }
    }
  }
</script>
