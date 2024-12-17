<template>
  <div class="results">
    <div class="card">
      <h2>Experiment Results</h2>
      <table>
        <thead>
          <tr>
            <th>Question</th>
            <th>Format</th>
            <th>Selected Answer</th>
            <th>Correct?</th>
            <th>Time Taken (s)</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(response, index) in responses" :key="index">
            <td>{{ response.question }}</td>
            <td>{{ response.format }}</td>
            <td>{{ response.selectedAnswer }}</td>
            <td :class="{ correct: response.isCorrect, incorrect: !response.isCorrect }">
              {{ response.isCorrect ? 'Yes' : 'No' }}
            </td>
            <td>{{ response.timeTaken }}</td>
          </tr>
        </tbody>
      </table>
      <div class="actions">
        <button @click="restartExperiment">Restart Experiment</button>
        <button @click="downloadCSV">Download CSV</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ResultsPage',
  props: {
    responses: {
      type: Array,
      required: true,
    },
  },
  methods: {
    restartExperiment() {
      this.$emit('restart'); // Emit event to reset the app and restart
    },
    downloadCSV() {
      // Prepare CSV header and rows
      const csvHeader = ['Question,Format,Selected Answer,Correct,Time Taken (s)'];
      const csvRows = this.responses.map((response) =>
        [
          `"${response.question}"`,
          response.format,
          `"${response.selectedAnswer}"`,
          response.isCorrect ? 'Yes' : 'No',
          response.timeTaken,
        ].join(',')
      );

      // Combine header and rows
      const csvContent = [csvHeader, ...csvRows].join('\n');

      // Create a Blob and download the file
      const blob = new Blob([csvContent], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.download = 'experiment_results.csv';
      link.click();
      URL.revokeObjectURL(url); // Cleanup
    },
  },
};
</script>


<style scoped>
.results {
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
  max-width: 800px;
  padding: 20px 30px;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  text-align: center;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
}

thead th {
  background-color: #764ba2;
  color: white;
  padding: 10px;
  text-align: left;
}

tbody td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

tbody td.correct {
  color: green;
  font-weight: bold;
}

tbody td.incorrect {
  color: red;
  font-weight: bold;
}

.actions {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 10px;
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
