
<script setup>
import { ref, reactive} from 'vue'
import { _ } from 'lodash'
var color = ref("")
const code = []
var answers = reactive([]);
var isRight = ref(false);
var realAnswer = ref("");
var clicked;

// Return a random hex color code
function randomColor(){
    for (let i = 0; i < 6; i++){
        code[i] = Math.round((Math.random() * (15-1) + 1))
    }
    
    for (let j = 0; j < 6; j++){
        if (code[j] > 9){
            switch (code[j]){
                case 10:
                    code[j] = 'A'
                    break;
                case 11:
                    code[j] = 'B'
                    break;
                case 12:
                    code[j] = 'C'
                    break;
                case 13:
                    code[j] = 'D'
                    break;
                case 14:
                    code[j] = 'E'
                    break;
                case 15:
                    code[j] = 'F'             
                    break;
            }
        }
    }
    
    color = code.toString().split(",").join("");
    return "#" + color;
}

// Check if user is correct, then reset answers
function checkColor(solution){
    clicked = true;
    
    // set isRight = true is user is right, otherwise false
    isRight = (solution === realAnswer);
    
    // reset answers
    setAnswers();
}

// Randomly assign the correct and decoy colors to elements of answers[]
function setAnswers() {
    realAnswer = randomColor();

    var indices = _.shuffle([0, 1, 2]); 

    answers[indices[0]] = realAnswer
    answers[indices[1]] = randomColor()
    answers[indices[2]] = randomColor()
}


// Create initial answers
setAnswers();
</script>

<template>
    <div id="app-container" :style="{backgroundColor: realAnswer}">
        <h2 id="title">COLOR GUESSER</h2>
        <div>
                <button @click="checkColor(answers[0])">{{ answers[0] }}</button>
                <button @click="checkColor(answers[1])">{{ answers[1] }}</button>
                <button @click="checkColor(answers[2])">{{ answers[2] }}</button>
        </div>
        
        <!-- div wont display until a button is clicked-->
        <div v-if="clicked"  id="answer-container">
            <h2>{{ isRight ? "Correct!" : "Wrong.." }}</h2>
        </div>
    </div>
</template>


<style scoped>

    #answer-container{
        background-color: white;
        width:10%;
        text-align: center;
        font-family: 'Courier New', monospace;
        border-radius:10px;
        font-size:20px;
        font-weight:bolder;
        margin-top:40px;
    }
    #app-container{
        margin-left:-260px;
        padding-bottom:20%;
        width:100vw;
        height:100vh;
        display:grid;
        grid-template: 1fr / 1fr;
        place-items:center;
        position:block;
        overflow: hidden;
    }

    #coloBox, #button{
        grid-column: 1 / 1;
        grid-row: 1 / 1;
    }

    #colorBox{
        z-index:2;
        width:100px;
        height:100px;
        margin:auto;
    }

    button{
        border-radius:5px;
        z-index:1;
        margin:20px;
        height:70px;
        width:135px;
    }

    #title{
        font-family: 'Courier New', monospace;
        font-size:50px;
        font-weight: bolder;
        background-color: aliceblue;
        width:20%;
        text-align: center;
        margin-bottom:-70px;
        border-radius: 50px;
    }
</style>