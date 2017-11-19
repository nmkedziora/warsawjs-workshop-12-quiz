<template>
  <v-app dark>
    <v-content>
      <v-container>
        <h1>warsawjs-workshop-12-quiz</h1>
        <v-layout row justify-center>
          <v-flex md6 xs10>
            <v-card v-if="hasQuizEnded">
              <v-card-media src="https://media1.tenor.com/images/4f586b8d5cdc536ada9889b58e6d91e8/tenor.gif?itemid=5131908" height="300px">
              </v-card-media>
              <v-card-title primary-title>
                <div class="text-xs-center">
                  <h3 class="headline mb-">congratulations!</h3>
                  <p>kim jesteś? jesteś zwycięzcą!</p>
                </div>
              </v-card-title>
            </v-card>
            <v-card v-else>
              <h5>Question {{ currentQuestionIndex + 1 }} / {{ quiz.length }}</h5>
              <v-card-title> {{ currentQuestion.title }}</v-card-title>
              <v-card-text>
                <v-list>
                  <v-list-tile
                    class="secondary"
                    v-for="(answer, index) in currentQuestion.answers"
                    :key="index"
                    v-on:click="saveAnswer(index)"
                    v-bind:class="getClass(index)"
                    >
                    {{ answer }}
                  </v-list-tile>
                </v-list>
              </v-card-text>
              <v-card-actions>
                <v-btn v-if="isCorrectAnswer && !hasQuizEnded" v-on:click="getNextQuestion">
                  następne pytanie
                </v-btn>
                <v-btn v-if="!isCorrectAnswer && !hasQuizEnded && userAnswer !== null" v-on:click="restart">
                  zacznij od początku
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-flex>
          <v-flex md4>
            <add-question
              @new-question-added="addQuestion($event)"
              :numberOfQuestions="quiz.length"
            >
            </add-question>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  import { quiz } from './quiz'
  import AddQuestion from './components/AddQuestion.vue'

  export default {
    components: {
      'add-question': AddQuestion
    },
    data () {
      return {
        quiz: quiz,
        currentQuestionIndex: 0,
        userAnswer: null,
        newQuestion: {}
      }
    },
    computed: {
      currentQuestion () {
        return this.quiz[this.currentQuestionIndex]
      },
      isCorrectAnswer () {
        return this.quiz[this.currentQuestionIndex].correctAnswerIndex === this.userAnswer
      },
      hasQuizEnded () {
        return this.isCorrectAnswer && (this.currentQuestionIndex === this.quiz.length - 1)
      }
    },
    created () {
      this.$http.get('https://opentdb.com/api.php?amount=10').then(response => {
        const data = response.body.results
        
        this.quiz = data.map(question => {
          return {
            title: question.question,
            answers: [...question.incorrect_answers, question.correct_answer],
            correctAnswerIndex: 3
          }
        })
      })
    },
    methods: {
      addQuestion (question) {
        this.quiz.unshift(question)
      },
      saveAnswer (index) {
        if (this.userAnswer === null) {
          this.userAnswer = index
        }
      },
      getClass (index) {
        return {
          success: this.userAnswer === index && this.userAnswer === this.currentQuestion.correctAnswerIndex,
          error: this.userAnswer === index && this.userAnswer !== this.currentQuestion.correctAnswerIndex
        }
      },
      getNextQuestion () {
        this.currentQuestionIndex++
        this.userAnswer = null
      },
      restart () {
        this.currentQuestionIndex = 0
        this.userAnswer = null
      }
    }
  }
</script>
