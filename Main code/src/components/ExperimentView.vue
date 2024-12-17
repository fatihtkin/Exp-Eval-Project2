<template>
  <div class="experiment">
    <div class="card">
      <h2>Question {{ questionIndex + 1 }}</h2>
      <p>Sentence: <strong>{{ question.sentence }}</strong></p>
      <p><strong>Format:</strong> {{ question.format }}</p>
      <div class="options">
        <button
          v-for="(option, index) in question.distractors"
          :key="index"
          @click="selectOption(option)"
        >
          {{ option }}
        </button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'ExperimentView',
  props: {
    question: {
      type: Object,
      required: true,
    },
    questionIndex: {
      type: Number,
      required: true,
    },
  },
  methods: {
    selectOption(option) {
      const isCorrect = option === this.question.correctAnswer;
      this.$emit('response', { selectedAnswer: option, isCorrect });
    },
  },
  mounted() {
    this.$emit('start-timer'); // Emit event to start the timer
  },
};
</script>



<style scoped>
.experiment {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea, #764ba2);
  font-family: 'Arial', sans-serif;
  color: white;
}

.card {
  background: white;
  color: #333;
  max-width: 600px;
  padding: 20px 30px;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  text-align: center;
}

h2 {
  color: #764ba2;
}

.options {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin-top: 20px;
}

button {
  background-color: #764ba2;
  color: white;
  font-size: 1rem;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #5a3f8a;
}
</style>
