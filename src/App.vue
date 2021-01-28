<template>
  <div id="app">
    <Header
      :correctAnswers="correctAnswers"
      :total="total"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctAnswers:0,
      total:0
    };
  },
  methods: {
    next: function () {
      this.index++;
    },
    increment(isCorrect){
      if(isCorrect)
        this.correctAnswers++;
      this.total++;
    }
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get",
    })
      .then((response) => response.json())
      .then((jsonData) => (this.questions = jsonData.results))
  },
};
</script>

<style>
</style>
