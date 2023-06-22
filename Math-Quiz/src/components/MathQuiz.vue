<template>
  <div id="Quiz_wrapper">
    <div id="Quiz_menu">
      <div class="Quiz_menu_selection" @click="start_quiz('+')">Addition</div>
      <div class="Quiz_menu_selection" @click="start_quiz('-')">Subtraction</div>
      <div class="Quiz_menu_selection" @click="start_quiz('*')">Multiplication</div>
      <div class="Quiz_menu_selection" @click="start_quiz('/')">Division</div>
    </div>
    <div id="Quiz_body">
      <div id="Quiz_streak">{{streak}}</div>
      <div id="Quiz_equation">{{(equation||"Choose a Quiz")}}</div>
      <div id="Quiz_answers" ref="for_parent">
        <div class="Quiz_answer" v-for="(answer, index) in answers" :key="index" @click="check_answer($event.target,answer)">{{answer}}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {ref} from "vue";

  const equation = ref("")
  const answers = ref([])
  const streak = ref(0)
  const position = ref(null)

  const for_parent = ref()
  let correct_answer = null
  let operator_selected = ''
  let answered = false


  const get_random_num = (max) => Math.ceil(Math.random()*max)

  //According to the internet (https://stackoverflow.com/questions/11832914/how-to-round-to-at-most-2-decimal-places-if-necessary)
  // this solution might not always be correct and one might argue that is too imprecise but for this project it will suffice
  const round_for_div = (num) => Math.round((num + Number.EPSILON) * 100) / 100

  const get_equation = (operator) =>`${get_random_num(20)}${operator}${get_random_num(20)}`

  const start_quiz = (operator) => {
    operator_selected = operator
    equation.value = get_equation(operator)
    get_answers(operator)
  }

  //Eval is bad practice but as security is not relevant I will use it here
  const get_answers = (operator) =>{
    answers.value = []
    correct_answer = round_for_div(eval(equation.value))
    position.value = get_random_num(4)
    let temp_answers = []
    for (let i = 0; i < 5; i++) {
      if (i === position.value) {
        temp_answers.push(correct_answer)
        continue
      }
      let answer = round_for_div(eval(get_equation(operator)))
      while (answer === correct_answer || temp_answers.indexOf(answer) !== -1){
        answer = round_for_div(eval(get_equation(operator)))
      }
      temp_answers.push(answer)
    }
    answers.value = temp_answers
  }

  const check_answer = (e,answer) =>{
    if(answered){return}
    if(answer === correct_answer){
      e.style.background = "green"
      streak.value++
    }else{
      e.style.background = "red"
      for_parent.value.children[position.value].style.background = "green"
      streak.value = 0

    }
    setTimeout(() => {
      e.style.background = "var(--steel)"
      for_parent.value.children[position.value].style.background = "var(--steel)"
      answered = false
      start_quiz(operator_selected)
    }, 500)

  }
</script>

<style scoped>

  #Quiz_wrapper{
    background: lightsteelblue;
    border: var(--steel) 2px solid;
    box-shadow: 0 0 2px 2px var(--steel);
    color: black;
    /*padding: 10px;*/
    display: flex;
    flex-direction: column;
    font-size: 2rem;
    width: 1280px;
    height: auto;
  }
  #Quiz_menu{
    height: 110px;
    background: var(--steel);
    margin-bottom: 10px;
    display: flex;
  }
  .Quiz_menu_selection{
    position: relative;
    border: 1px solid lightsteelblue;
    margin-bottom: 5px;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: all .2s ease-in-out;
  }
  .Quiz_menu_selection:hover{
    background: #8da1bb;
  }

  #Quiz_body{
    height: 400px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
  }
  #Quiz_answers{
    display: grid;
    grid-template-columns: repeat(5, 1fr);
  }
  .Quiz_answer{
    min-width: 100px;
    width: auto;
    height: 100px;
    border: 1px solid lightsteelblue;
    cursor: pointer;
    background: var(--steel);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: all .2s ease-in-out;
  }
  .Quiz_answer:hover{
    background: #8da1bb;
  }
  #Quiz_streak{
    position: relative;
    place-self: end;
    width: auto;
    height: 50px;
    text-align: center;
    background: var(--steel);
    color: gold;
    min-width: 50px;
  }

</style>