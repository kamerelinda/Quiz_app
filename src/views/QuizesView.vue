<script setup>
  import q from "../data/data.json"
  import {ref, watch} from "vue"
  import Card from "../components/Card.vue"
  import gsap from 'gsap'

  const quizes = ref(q)
  const search = ref("")

  watch(search, () => {
    quizes.value = q.filter(quiz => quiz.name.toLowerCase().includes(search.value.toLowerCase()))
  })

  const beforeEnter = (el) => {
    el.style.opacity = 0;
    console.log("BEFORE I ENTER")
  }
  const enter = (el) => {
    gsap.to(el,{
      y: 0,
      opacity: 1,
      duration: 0.5,
      // renders every element with a delay of 1 * 0.3, 2 * 0.3
      delay: el.dataset.num * 0.3
    })
    console.log("Enter")
  }
  const afterEnter = () => {
    console.log("AFTER I ENTER")
  }


</script>

<template>
  <div>
    <header>
      <h1>Quizes</h1>
      <input v-model.trim="search" type="text" placeholder="Search...">
    </header>
    <div class="options-container">
      <transition-group
          appear
          @before-enter="beforeEnter"
          @enter="enter"
          @after-enter="afterEnter" >
        <Card
            v-for="(quiz, index) in quizes"
            :key="quiz.id"
            :quiz="quiz"
            :data-num="index"
             />
      </transition-group>
      <!-- <div v-for="quiz in quizes" :key="quiz.id" class="card">
        <img :src="quiz.img" alt="">
        <div class="card-text">
          <h2>{{ quiz.name }}</h2>
          <p>{{quiz.questions.length}} questions</p>
        </div>
      </div> -->
    </div>
  </div>
</template>

<style scoped>
  

  header {
    margin-bottom: 10px;
    margin-top: 30px;
    display: flex;
    align-items: center;
  }

  header h1 {
    font-weight: bold;
    margin-right: 30px;
  }

  header input {
    border: none;
    background-color: rgba(128,128,128,0.1);
    padding: 10px;
    border-radius: 5px;
  }

  .options-container {
    display: flex;
    flex-wrap: wrap;
    margin-top: 40px;
  }
  /*.card-enter-from{

  }
  .card-enter-to {
    opacity: 1;
    transform: translateY(0px);
  }
  .card-enter-active {
    transition: all 5s ease ;
  }
  /*.card-leave-from{
    opacity: 1;
    transform: translateY(0px);
  }
  .card-leave-to {
    opacity: 0;
    transform: translateY(-50px);
  }
  .card-leave-active {
    transition: all 5s ease ;
  }*/
</style>