<template>
  <v-app dark>
    <v-content>
      <v-container>
        <v-layout>
          <v-flex>
            <h1>warsawjs-workshop-12-quiz</h1>
            <v-card v-if="hasQuizEnded">
              <v-card-media src="https://media1.tenor.com/images/4f586b8d5cdc536ada9889b58e6d91e8/tenor.gif?itemid=5131908" height="300px">
              </v-card-media>
              <v-card-title primary-title>
                <div class="text-xs-center">
                  <h3 class="headline mb-">congratulations!</h3>
                  <p>jesteś zwycięzcą</p>
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
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  import { quiz } from './quiz'
  export default {
    data () {
      return {
        quiz: quiz,
        currentQuestionIndex: 0,
        userAnswer: null
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
    methods: {
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

<style></style>
