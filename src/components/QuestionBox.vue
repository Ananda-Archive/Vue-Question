<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestionEncoded }}
            </template>
            <hr class="my-4">
            <b-list-group>
                <b-list-group-item v-for="(answer,idx) in list" :key="idx" @click="selectAnswer(idx)" :class="[selectedIndex === idx ? 'selected' : '']">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>
            <br>
            <b-button variant="primary" href="#" @click="submitAnswer(selectedIndex)" :disabled="selectedIndex == null">Submit</b-button> &emsp;
            <b-button variant="success" href="#" @click="next" :disabled="done.length==questionLength-1">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>

import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        index: Number,
        done: Array,
        questionLength: Number
    },
    data() {
        return {
            selectedIndex: null, // selected answer
            list: [], // list of answer
            corrected: 0,
            currentQuestionEncoded: ''
        }
    },
    // Computed can't change variable values, so we use watch here for resetting index
    computed: {
        listOfAnswers() {
            let list = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            return list
        }
    },
    methods: {
        selectAnswer(idx) {
            this.selectedIndex = idx
        },
        shuffleAns() {
            let list = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            this.list = _.shuffle(list)
        },
        encodingEntities(str) {
            let txt = document.createElement('textarea')
            txt.innerHTML = str
            this.currentQuestionEncoded = txt.value
        },
        submitAnswer(selectedIndex) {
            if(this.done.length != this.questionLength) {
                if(this.list[selectedIndex] == this.currentQuestion.correct_answer) {
                    this.corrected++
                    this.$emit('correct',this.corrected)
                }
                this.done.push(this.index)
                this.next()
            } else {
                //finish the question, maybe a congrats??
            }
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.encodingEntities(this.currentQuestion.question)
                this.currentQuestionEncoded
                this.shuffleAns()
            }
        }
    }
}

</script>

<style scoped>

.list-group-item:hover{
    /* background-color: #EEE; */
    cursor: pointer;
}

.selected{
    background-color: aqua;
}

.correct{
    background-color: green;
}

.wrong{
    background-color: red;
}


</style>