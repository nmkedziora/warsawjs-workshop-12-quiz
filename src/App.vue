<template>
  <v-app dark>
    <v-content>
      <v-container>
        <v-layout>
          <v-flex>
            <v-card>
              <h1>WarsawJS Quiz</h1>
              <h5>Question #{{ currentQuestionIndex + 1 }}</h5>
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
                <v-btn v-if="isCorrectAnswer" v-on:click="getNextQuestion">
                  następne pytanie
                </v-btn>
                <v-btn v-if="userAnswer && !isCorrectAnswer" v-on:click="restart">
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
