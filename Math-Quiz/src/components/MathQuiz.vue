<template>
  <div id="Quiz_wrapper">
    <div id="Quiz_menu">
      <div class="Quiz_menu_selection">Addition</div>
      <div class="Quiz_menu_selection">Subtraction</div>
      <div class="Quiz_menu_selection">Multiplication</div>
      <div class="Quiz_menu_selection">Division</div>
    </div>
    <div id="Quiz_body">
      <div id="Quiz_streak">{{streak}}</div>
      <div id="Quiz_equation">{{equation}}</div>
      <div id="Quiz_answers">
        <div class="Quiz_answer" v-for="answer in answers" :key="answer">{{answer}}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {ref} from "vue";

  const equation = ref("69+420")
  const answers = ref([1,2,3,4,5])
  const streak = ref(0)


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