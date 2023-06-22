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
      <!--   The solution with setting the id is not very smart but will suffice for this project!   -->
      <div id="Quiz_answers">
        <div class="Quiz_answer" v-for="(answer, index) in answers" :key="answer" @click="check_answer" :id="position === index ? 'correct' : ''" :ref="(el) => functionRef(el, index)">{{answer}}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {ref} from "vue";
  const correct = ref(null)
  const equation = ref("")
  const answers = ref([])
  const streak = ref(0)
  const position = ref(null)

  let operator_selected = ''
  let answered = false

  const functionRef = (el, index) => {
    if(index === position.value){
      correct.value = el
    }
  }

  const get_random_num = (max) => Math.ceil(Math.random()*max)

  //According to the internet (https://stackoverflow.com/questions/11832914/how-to-round-to-at-most-2-decimal-places-if-necessary)
  // this solution might not always be correct and one might argue that is too imprecise but for this project it will suffice
  const round_for_div = (num) => Math.round((num + Number.EPSILON) * 100) / 100

  const get_equation = (operator) =>`${get_random_num(20)}${operator}${get_random_num(20)}`

  const start_quiz = (operator) => {
    operator_selected = operator
    equation.value = ""
    equation.value = get_equation(operator)
    get_answers(operator)
  }


  const get_answers = (operator) =>{
    answers.value = []
    const correct_ = round_for_div(eval(equation.value))
    position.value = get_random_num(4)

    for (let i = 0; i < 5; i++) {
      if (i === position.value) {
        answers.value.push(correct_)
        continue
      }
      let answer = round_for_div(eval(get_equation(operator)))
      while (answer === correct_ || answers.value.indexOf(answer) !== -1){
        answer = round_for_div(eval(get_equation(operator)))
      }
      answers.value.push(answer)
    }
  }

  const check_answer = (e) =>{
    if(answered){return}
    answered = true
    if(e.target.id === 'correct'){
      e.target.style.background = "green"
      streak.value++
    }else{
      e.target.style.background = "red"
      correct.value.style.background = "green"
      streak.value = 0
    }
    setTimeout(() => {
      e.target.style.background = "var(--steel)"
      correct.value.style.background = "var(--steel)"
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