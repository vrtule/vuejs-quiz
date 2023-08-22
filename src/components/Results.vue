<template>
  <div class="mb-4">
    <h2 class="mb-2">Tvoje výsledky</h2>
    <div class="grid grid-cols-2 gap-4">
      <div>
        <p>
          Správných odpovědí: {{ correctAnswers }}
        </p>
      </div>
      <div>
        <p>Nesprávných odpovědí: {{ totalQuestions - correctAnswers }}</p>
      </div>
      <div>
        <p>Otázek celkem: {{ totalQuestions }}</p>
      </div>
      <div>
        <p>Úspěšnost: {{ Math.trunc(correctAnswers / totalQuestions * 100) }}%</p>
      </div>
      <div>
        <p>Čas: {{ formatMilliseconds(quizTime) }}</p>
      </div>
    </div>
    <div class="mb-4">
      <h2 class="mb-2">Nejlepší výsledky</h2>
      <table class="w-full border-collapse border border-gray-300">
        <thead>
        <tr>
          <th class="border p-2">Pořadí</th>
          <th class="border p-2">Úspěšnost</th>
          <th class="border p-2">Čas</th>
        </tr>
        </thead>
        <tbody>
        <tr
            v-for="(item, index) in dashboardData"
            :key="index"
            :class="item.isNew ? 'bg-blue-500' : ''"
        >
          <td class="border p-2">{{ index + 1 }}.</td>
          <td class="border p-2">{{ Math.trunc(item.points / totalQuestions * 100) }}%</td>
          <td class="border p-2">{{ formatMilliseconds(item.time) }}</td>
        </tr>
        </tbody>
      </table>
    </div>
    <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded"
        @click="startAgain"
    >
      Začít znovu
    </button>
  </div>
</template>

<script>
export default {
  props: {
    correctAnswers: Number,
    totalQuestions: Number,
    quizTime: Number,
    dashboardData: Array
  },

  methods: {
    formatMilliseconds(milliseconds) {
      const totalSeconds = Math.floor(milliseconds / 1000);
      const hours = Math.floor(totalSeconds / 3600);
      const minutes = Math.floor((totalSeconds % 3600) / 60);
      const seconds = totalSeconds % 60;

      const formattedHours = hours.toString().padStart(2, '0');
      const formattedMinutes = minutes.toString().padStart(2, '0');
      const formattedSeconds = seconds.toString().padStart(2, '0');

      if (hours === 0) {
        return `${formattedMinutes}:${formattedSeconds}`;
      } else {
        return `${formattedHours}:${formattedMinutes}:${formattedSeconds}`;
      }
    },

    startAgain() {
      this.$emit('startAgain');
    }
  }
};
</script>
