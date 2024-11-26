<template>
  <div id="app">
    <transition name="fade" mode="out-in">
      <div key="page">
        <!-- Navigation between components -->
        <WelcomePage
          v-if="currentPage === 'welcome'"
          @start-experiment="goToDemographics"
        />
        <DemographicsForm
          v-if="currentPage === 'demographics'"
          @form-submitted="goToExperiment"
        />
        <ExperimentView
          v-if="currentPage === 'experiment'"
          :question="questions[currentQuestionIndex]"
          :questionIndex="currentQuestionIndex"
          @response="handleResponse"
          @start-timer="startTimer"
        />
        <ResultsPage
          v-if="currentPage === 'results'"
          :responses="responses"
          @restart="restartExperiment"
        />
      </div>
    </transition>
  </div>
</template>

<script>
import WelcomePage from './components/WelcomePage.vue';
import DemographicsForm from './components/DemographicsForm.vue';
import ExperimentView from './components/ExperimentView.vue';
import ResultsPage from './components/ResultsPage.vue';

export default {
  name: 'App',
  components: {
    WelcomePage,
    DemographicsForm,
    ExperimentView,
    ResultsPage,
  },
  data() {
    return {
      currentPage: 'welcome', // Tracks the current page
      currentQuestionIndex: 0, // Tracks the index of the current question
      responses: [], // Stores user responses
      questionsStartTime:null,
      originalQuestions: [
      {
        sentence: "move north",
        distractors: ["move-north", "move-source", "mover-north", "move-sound"],
        correctAnswer: "move-north",
        format: "kebab-case",
      },
      {
        sentence: "move north",
        distractors: ["moveNorth", "moveSource", "moverNorth", "moveSound"],
        correctAnswer: "moveNorth",
        format: "camel-case",
      },
      {
        sentence: "jump high",
        distractors: ["jump-high", "jump-height", "jumb-high", "jump-hug"],
        correctAnswer: "jump-high",
        format: "kebab-case",
      },
      {
        sentence: "jump high",
        distractors: ["jumpHigh", "jumpHeight", "jumbHigh", "jumpHug"],
        correctAnswer: "jumpHigh",
        format: "camel-case",
      },
      {
        sentence: "run fast",
        distractors: ["run-fast", "fun-fast", "run-past", "ran-fast"],
        correctAnswer: "run-fast",
        format: "kebab-case",
      },
      {
        sentence: "run fast",
        distractors: ["runFast", "funFast", "runPast", "ranFast"],
        correctAnswer: "runFast",
        format: "camel-case",
      },
      {
        sentence: "cook dinner",
        distractors: ["cook-dinner", "book-dinner", "look-dinner", "cook-diner"],
        correctAnswer: "cook-dinner",
        format: "kebab-case",
      },
      {
        sentence: "cook dinner",
        distractors: ["cookDinner", "bookDinner", "lookDinner", "cookDiner"],
        correctAnswer: "cookDinner",
        format: "camel-case",
      },
      {
        sentence: "build house",
        distractors: ["build-house", "built-house", "build-hose", "burn-house"],
        correctAnswer: "build-house",
        format: "kebab-case",
      },
      {
        sentence: "build house",
        distractors: ["buildHouse", "builtHouse", "buildHose", "burnHouse"],
        correctAnswer: "buildHouse",
        format: "camel-case",
      },
      {
        sentence: "close all windows",
        distractors: [
          "close-all-windows",
          "chose-all-windows",
          "close-gall-windows",
          "close-all-wind",
        ],
        correctAnswer: "close-all-windows",
        format: "kebab-case",
      },
      {
        sentence: "close all windows",
        distractors: [
          "closeAllWindows",
          "choseAllWindows",
          "closeGallWindows",
          "closeAllWind",
        ],
        correctAnswer: "closeAllWindows",
        format: "camel-case",
      },
      {
        sentence: "send a quick message",
        distractors: [
          "send-a-quick-message",
          "sent-a-quick-message",
          "send-a-quit-message",
          "send-a-quack-message",
        ],
        correctAnswer: "send-a-quick-message",
        format: "kebab-case",
      },
      {
        sentence: "send a quick message",
        distractors: [
          "sendAQuickMessage",
          "sentAQuickMessage",
          "sendAQuitMessage",
          "sendAQuackMessage",
        ],
        correctAnswer: "sendAQuickMessage",
        format: "camel-case",
      },
      {
        sentence: "eat a red apple",
        distractors: [
          "eat-a-red-apple",
          "eat-a-read-apple",
          "eat-a-reed-apple",
          "aet-a-red-apple",
        ],
        correctAnswer: "eat-a-red-apple",
        format: "kebab-case",
      },
      {
        sentence: "eat a red apple",
        distractors: [
          "eatARedApple",
          "eatAReadApple",
          "eatAReedApple",
          "aetARedApple",
        ],
        correctAnswer: "eatARedApple",
        format: "camel-case",
      },
      {
        sentence: "scan the printed document",
        distractors: [
          "scan-the-printed-document",
          "scam-the-printed-document",
          "scan-the-printed-doctrine",
          "scan-the-printer-document",
        ],
        correctAnswer: "scan-the-printed-document",
        format: "kebab-case",
      },
      {
        sentence: "scan the printed document",
        distractors: [
          "scanThePrintedDocument",
          "scamThePrintedDocument",
          "scanThePrintedDoctrine",
          "scanThePrinterDocument",
        ],
        correctAnswer: "scanThePrintedDocument",
        format: "camel-case",
      },
      {
        sentence: "watch a new movie",
        distractors: [
          "watch-a-new-movie",
          "wash-a-new-movie",
          "watch-a-few-movie",
          "watch-a-news-movie",
        ],
        correctAnswer: "watch-a-new-movie",
        format: "kebab-case",
      },
      {
        sentence: "watch a new movie",
        distractors: [
          "watchANewMovie",
          "washANewMovie",
          "watchAFewMovie",
          "watchANewsMovie",
        ],
        correctAnswer: "watchANewMovie",
        format: "camel-case",
      },
    ],
    questions: [],
    };
  },
  methods: {
    // Shuffle function using the Fisher-Yates algorithm
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]]; // Swap elements
      }
      return array;
    },

    // Start the timer for the current question
    startTimer() {
      this.questionsStartTime = Date.now(); // Set the start time when a question is displayed
    },

    // Navigate to the demographics page
    goToDemographics() {
      this.currentPage = 'demographics';
    },

    // Navigate to the experiment page and shuffle the questions
    goToExperiment(demographics) {
      console.log('Demographics:', demographics); // Log demographic data

      // Reset and shuffle questions
      this.questions = this.originalQuestions.map((question) => ({
        ...question,
        distractors: this.shuffleArray([...question.distractors]), // Create a new shuffled array
      }));

      this.questionsStartTime = Date.now(); // Start the timer for the first question
      this.currentPage = 'experiment'; // Transition to the experiment page
    },

    // Handle the response for a question
    handleResponse({ selectedAnswer, isCorrect }) {
      const elapsedTime = Date.now() - this.questionsStartTime; // Calculate elapsed time

      // Store the response
      this.responses.push({
        question: this.questions[this.currentQuestionIndex].sentence,
        format: this.questions[this.currentQuestionIndex].format,
        selectedAnswer,
        isCorrect,
        timeTaken: (elapsedTime / 1000), // Convert to seconds with 2 decimals
      });

      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex += 1; // Move to the next question
        this.questionsStartTime = Date.now(); // Reset the timer for the next question
      } else {
        this.currentPage = 'results'; // Transition to the results page
      }
    },

    // Restart the experiment
    restartExperiment() {
      this.currentPage = 'welcome'; // Reset to the welcome page
      this.currentQuestionIndex = 0; // Reset the question index
      this.responses = []; // Clear all previous responses

      // Reset and shuffle questions
      this.questions = this.originalQuestions.map((question) => ({
        ...question,
        distractors: this.shuffleArray([...question.distractors]),
      }));
    },
  },

  // Lifecycle hook to initialize questions with shuffled distractors on app load
  created() {
    this.questions = this.originalQuestions.map((question) => ({
      ...question,
      distractors: this.shuffleArray([...question.distractors]), // Shuffle distractors
    }));
  },

};
</script>


<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
