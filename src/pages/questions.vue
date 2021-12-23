<template>
  <div id="app">
    <!-- <h1>{{ quiz.title }}</h1> -->
    <!-- index is used to check with current question index -->
    <div v-for="(question, index) in quiz.questions" :key="index" class="quiz__card">
      <!-- Hide all questions, show only the one with index === to current question index -->
      <div v-show="index === questionIndex" class="shadow-sm bg-white mx-auto question__card">
        <h2 class="bg-main p-2 text-white text-center">{{ question.text }}</h2>
        <ul>
          <li v-for="(response, id) in question.responses" :key="id" class="p-4">
            <label>
              <!-- The radio button has three new directives -->
              <!-- v-bind:value sets "value" to "true" if the response is correct -->
              <!-- v-bind:name sets "name" to question index to group answers by question -->
              <!-- v-model creates binding with userResponses -->
              <input
                type="radio"
                v-bind:value="response.correct"
                v-bind:name="index"
                v-model="userResponses[index]"
              />
              {{ response.text }}
            </label>
          </li>
        </ul>
        <!-- The two navigation buttons -->
        <!-- Note: prev is hidden on first question -->
        <div class="text-center p-3 ">
          <button class="btn bg-red mr-3" v-show="questionIndex > 0" v-on:click="prev">
          Prev
        </button>
        <button class="btn bg-main" v-on:click="next" v-show="questionIndex >= 0">
          Next 
        </button>
        <button class="btn bg-main" v-show="questionIndex === quiz.questions.length"  v-on:click="finish">
          Finish
        </button>
        </div>
        
      </div>
    </div>
    <div v-show="questionIndex === quiz.questions.length">
      <h2>
        Quiz finished
      </h2>
      <p>Total score: {{ score() }} / {{ quiz.questions.length }}</p>
    </div>
  </div>
</template>



<style scoped>
*{
  padding: 0;
  margin: 0
}
#app {
  background: #FBBB07;
  display: flex;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
}
.question__card{
  border-radius: 15px;
  width: 80%;
  
}
.quiz__card ul li {
list-style: none;
}
.btn {
  padding: 0.5rem 2rem;
  font-weight: 500;
  color: #fff;
}
.btn:hover {
  background: #fff;
  border: 0px;
  border-radius: 0px;
  border-bottom: 2px solid #3CAC70;
  transition: 1s ease-in-out;
  color: #3CAC70;
}
</style>



<script>
  import quiz from "@/assets/js/quiz.json";
  export default {
    data() {
      return {
        quiz: quiz,
        // Store current question index
        questionIndex: 0,
        // An array initialized with "false" values for each question
        // It means: "did the user answered correctly to the question n?" "no".
        userResponses: Array(quiz.questions.length).fill(false),
      };
      // The view will trigger these methods on click
    },
    methods: {
      // Go to next question
      next: function () {
        this.questionIndex++;
      },
      // Go to previous question
      prev: function () {
        this.questionIndex--;
      },
      finish: function(){
        this.$router.push('/result')
      },
      // Return "true" count in userResponses
      score: function () {
        return this.userResponses.filter(function (val) {
          return val;
        }).length;
      },
    },
  };
</script>