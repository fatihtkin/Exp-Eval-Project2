<template>
    <div class="welcome-page">
      <div class="card">
        <!-- First Page: Instructions -->
        <div v-if="currentPage === 1">
          <h1>Welcome to the Experiment!</h1>
          <p>
            Thank you for participating in our study on code readability. In this experiment, 
            we aim to investigate whether the format of identifiers in source code affects 
            reading speed and accuracy. Specifically, we are comparing two popular identifier 
            styles: <strong>camelCase</strong> and <strong>kebab-case</strong>.
          </p>
          <button @click="goToNextPage">Next</button>
        </div>
  
        <!-- Second Page: Examples -->
        <div v-if="currentPage === 2">
          <h1>Understanding camelCase and kebab-case</h1>
          <p>
            <strong>Kebab case</strong>: Written like-this-for-example, using <code>-</code> 
            instead of spaces. <br />
            <strong>Camel case</strong>: Uses capitalization, likeThisExample.
          </p>
          <p>
            Examples:
            <ul>
              <li><strong>camelCase</strong>: moveSouth, jumpHigh</li>
              <li><strong>kebab-case</strong>: move-south, jump-high</li>
            </ul>
          </p>
          <button @click="goToNextPage">Next</button>
        </div>
  
        <!-- Third Page: Instructions -->
        <div v-if="currentPage === 3">
          <h1>Instructions</h1>
          <p>
            You will be presented with a series of tasks where you need to identify the correct 
            version of a phrase represented in code. Each task consists of:
            <ol>
              <li>A short sentence, such as “move south,” displayed on the screen.</li>
              <li>A set of identifiers using camelCase or kebab-case, along with some distractors.</li>
            </ol>
            Your goal is to quickly and accurately select the identifier that matches the sentence shown earlier.
          </p>
          <button @click="goToNextPage">Next</button>
        </div>
  
        <!-- Fourth Page: Ready -->
        <div v-if="currentPage === 4">
          <h1>Ready to Begin?</h1>
          <p>
            When you are ready, press the <strong>“Start”</strong> button to proceed to the demographics form. 
            Thank you for your participation!
          </p>
          <button @click="startExperiment">Start</button>
        </div>
      </div>
    </div>
  </template>
  
  
  <script>
  export default {
    name: 'WelcomePage',
    data() {
      return {
        currentPage: 1, // Tracks the current section
      };
    },
    methods: {
      goToNextPage() {
        this.currentPage += 1; // Move to the next page
      },
      startExperiment() {
        this.$emit('start-experiment'); // Emit event to parent to proceed
      },
    },
  };
  </script>
  
  
  <style scoped>
.welcome-page {
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
  text-align: left;
  animation: fade-in 0.5s ease;
}

h1 {
  color: #764ba2;
}

p {
  font-size: 1rem;
  line-height: 1.6;
  margin-bottom: 1rem;
}

ul, ol {
  margin-left: 20px;
}

li {
  margin-bottom: 0.5rem;
}

code {
  background: #f4f4f4;
  padding: 2px 4px;
  border-radius: 3px;
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

@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
