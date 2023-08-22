<template>
  <div>
    <Question
        v-if="quizStarted && !quizEnded"
        :question="currentQuestion.question"
        :options="currentQuestion.options"
        :selectedAnswer="selectedAnswer"
        @selectAnswer="selectAnswer"
        @nextQuestion="nextQuestion"
    />
    <Results
        v-else-if="quizStarted && quizEnded"
        :correctAnswers="correctAnswers"
        :totalQuestions="quizData.length"
        :quizTime="quizTime"
        :dashboardData="dashboardData"
        @startAgain="startAgain"
    />
    <div v-else>
      <h1>Quiz</h1>
      <button
          type="button"
          class="bg-green-600 hover:bg-green-700 mt-3 text-white font-bold py-2 px-4 rounded"
          @click="startQuiz"
      >
        Začít
      </button>
    </div>
  </div>
</template>

<script>
import Question from './Question.vue';
import Results from './Results.vue';

export default {
  components: {
    Question,
    Results
  },

  props: {
    quizData: Array
  },

  data() {
    return {
      quizStarted: false,
      quizTime: 0,
      quizEnded: false,
      currentQuestionIndex: 0,
      correctAnswers: 0,
      selectedAnswer: null,
      startTime: null,
      dashboardData: []
    };
  },

  computed: {
    currentQuestion() {
      return this.quizData[this.currentQuestionIndex];
    }
  },

  methods: {
    startQuiz() {
      this.quizStarted = true;
      this.startTime = new Date();
      this.currentQuestionIndex = 0;
      this.correctAnswers = 0;
      this.selectedAnswer = null;
    },

    selectAnswer(answer) {
      this.selectedAnswer = answer;
    },

    nextQuestion() {
      if (this.selectedAnswer === this.currentQuestion.correct_answer) {
        this.correctAnswers++;
      }
      this.selectedAnswer = null;
      this.currentQuestionIndex++;

      if (this.currentQuestionIndex >= this.quizData.length) {
        this.finishQuiz();
      }
    },

    finishQuiz() {
      this.quizTime = new Date() - this.startTime;
      this.quizEnded = true;

      if (!localStorage.getItem('dashboard')) {
        const initialRecord = [{ 'time': this.quizTime, 'points': this.correctAnswers, 'isNew': true }];
        localStorage.setItem('dashboard', JSON.stringify(initialRecord));
      } else {
        const existingRecords = JSON.parse(localStorage.getItem('dashboard'));

        for (const record of existingRecords) {
          record.isNew = false;
        }

        existingRecords.push({ 'time': this.quizTime, 'points': this.correctAnswers, 'isNew': true });

        existingRecords.sort((a, b) => {
          if (a.points !== b.points) {
            return b.points - a.points;
          }
          return a.time - b.time;
        });

        const topRecords = existingRecords.slice(0, 5);

        localStorage.setItem('dashboard', JSON.stringify(topRecords));
      }

      this.dashboardData = JSON.parse(localStorage.getItem('dashboard'));
    },

    startAgain() {
      this.quizStarted = false;
      this.quizTime = 0;
      this.quizEnded = false;
      this.currentQuestionIndex = 0;
      this.correctAnswers = 0;
      this.selectedAnswer = null;
      this.startTime = null;
      this.dashboardData = [];
    }
  }
};
</script>
