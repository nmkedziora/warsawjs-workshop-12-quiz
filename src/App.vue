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
      }
    }
  }
</script>

<style></style>
