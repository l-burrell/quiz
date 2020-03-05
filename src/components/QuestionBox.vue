<template>
    <div id="question-box-container">
    <h3>{{currentQuestion.question}}</h3>
    <hr>
    <div id="question-box-answer-choices">
        <ul>
            <li v-for="(answer, index) in shuffledAnswers" :key="index" 
            @click="selectAnswer(index)"
            :class="answerClass(index)">{{answer}}</li>
        </ul>
        <div id="button-holder">
            <button @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</button>
            <button @click="next" :disabled="this.currentIndex === this.getTotal-1">Next Question</button>
            <p id="quiz-message" v-if="this.currentIndex === this.getTotal-1 && answered">You finished the entire quiz!</p>
        </div>
    </div>
    </div>
</template>

<script>
import _ from 'lodash';

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        getTotal: Function,
        currentIndex: Function,
        // numCorrect: Number,
    },
    data () {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered: false,
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.shuffleAnswers();
                this.answered = false;
            }
        },
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            return this.shuffledAnswers;
        },
        submitAnswer() {
            let isCorrect = false;
            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
        },  
        answerClass(index){
            let answerClass = ''

            if(!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if(this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                answerClass = 'incorrect'
            }
            return answerClass;
        }
    },
}
</script>

<style scoped>
h3 {
    text-align: center;
    font-size: 20px;
}
button {
    background-color: #333;
    color: white;
    font-weight: bold;
    border: none;
    height: 40px;
    margin: 5px 10px 5px 5px;
    cursor: pointer;
}
hr {
    color: rgba(1, 1, 1, .1)
}
#question-box-container {
    box-sizing: border-box;
    background-color: hsl(293, 55%, 87%);
    margin: 30px auto;
    max-width: 800px;
    padding: 20px;
}
#question-box-answer-choices ul {
    text-align: center;
}
#question-box-answer-choices ul li {
    list-style: none;
    padding: 20px;
    border-bottom: solid rgba(1, 1, 1, 0.2) 0.06px;
}
#question-box-answer-choices ul li:hover {
    background-color: rgb(209, 132, 203);
    cursor: pointer;
}
#button-holder {
    text-align: center;
    margin-left: 5pc;
}
#quiz-message {
    background-color: #333;
    color: white;
    z-index: 2;
    padding: 50px;
    position: absolute;
    margin-left: auto;
    margin-right: auto;
    left: 0;
    right: 0;
    top: 100px;
    width: 600px;
}
.selected {
    background-color: rgb(209, 132, 203);
}
.correct {
        background-color: rgb(40, 172, 57);
}
.incorrect {
        background-color: rgb(216, 59, 54);
}
</style>