<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Website's Logo</b></b-nav-item>
            <b-nav-item disabled>Score: {{ scoreCalculation }} </b-nav-item>
            <b-nav-item disabled>Answered: {{ done }} / {{ questionLength }} </b-nav-item>
        </b-nav>
    </div>
</template>

<script>

import _ from 'lodash'

export default {
    props: {
        latestScore: Number,
        questionLength: Number,
        done: Number
    },
    data() {
        return {
            score: 0.0
        }
    },
    computed: {
        scoreCalculation() {
            if(this.done == 0) {
                return 0
            } else {
                this.$emit('finalScore',_.round(this.latestScore/this.done*10,2))
                return _.round(this.latestScore/this.done*10,2)
            }
        }
    }

}
</script>