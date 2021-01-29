<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />
        <b-list-group>
            <b-list-group-item
            v-for="(answer, index) in answers"
            :key="index"
            @click.prevent="selectAnswer(index)"
            :class="answerClass(index)"
            >
              {{ answer }}
            </b-list-group-item>
        </b-list-group>
      
      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="next"
        variant="success">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment:Function
  },
  data(){
      return{
        selectedIndex:null,
        shuffledAnswers:[],
        correctIndex: null,
        answered:false,
      }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  methods:{
      selectAnswer(index){
          this.selectedIndex=index;
      },
      shuffleAnswers(){
        let answer=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
        this.shuffledAnswers= _.shuffle(answer);
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      submitAnswer(){
        let isCorret=false;
        if(this.selectedIndex === this.correctIndex)
          isCorret=true;
        this.increment(isCorret)
        this.answered=true;
      },
      answerClass(index){
        let answerClass='';
        if(!this.answered && this.selectedIndex === index)
          answerClass = 'selected';
        else if(this.answered && this.correctIndex === index)
          answerClass='correct';
        else if(this.answered && this.selectedIndex===index && this.correctIndex !== index)
          answerClass='wrong';
        return answerClass;
      }
  },
  watch:{
    currentQuestion:{
      immediate:true,
      handler(){
        this.answered=false;
        this.selectedIndex=null;
        this.shuffleAnswers();
      },
    }

  }
}
</script>

<style scoped>
    .list-group{
        margin-bottom:15px ;
    }

    .btn{
        margin:0 5px ;
    }

    .list-group-item:hover{
        background: #eee;
        cursor: pointer;
    }

    .selected{
        background:lightskyblue;
    }

    .correct{
        background: lightgreen;
    }

    .wrong{
        background: lightsalmon;
    }
</style>