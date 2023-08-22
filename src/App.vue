<template>
  <div id="app">
    <Quiz :quizData="quizData" />
  </div>
</template>

<script>
import axios from "axios";
import _ from "underscore";
import Quiz from './components/Quiz.vue';

export default {
  components: {
    Quiz
  },
  data() {
    return {
      quizData: []
    };
  },

  mounted() {
    axios.get('./questions.json')
        .then(response => {
          this.quizData = _.shuffle(response.data);
          console.log(this.quizData)
        })
        .catch(function (error) {
          console.log(error);
        });
  }
};
</script>


