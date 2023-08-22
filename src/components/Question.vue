<template>
  <div>
    <div class="mb-2">
      <h1>{{ question }}</h1>
    </div>
    <div class="mb-3">
      <div class="grid grid-cols-2 gap-4">
        <button
            type="button"
            class="p-4 block mb-2 rounded bg-green-500 hover:bg-green-700 font-bold"
            :class="selectedAnswer === item ? 'bg-green-700' : ''"
            v-for="(item, index) in options"
            :key="index"
            @click="select(item)"
        >
          {{ item }}
        </button>
      </div>
    </div>
    <div class="flex justify-between">
      <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded"
          @click="nextQuestion"
          :disabled="selectedAnswer === null"
      >
        Další
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    question: String,
    options: Array,
    selectedAnswer: String
  },
  methods: {
    select(answer) {
      this.$emit('selectAnswer', answer);
    },

    nextQuestion() {
      this.$emit('nextQuestion');
    }
  }
};
</script>
