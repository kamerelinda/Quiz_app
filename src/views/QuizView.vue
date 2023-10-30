<script setup>
    import Question from '../components/Question.vue';
    import QuizHeader from '../components/QuizHeader.vue'
    import { useRoute } from 'vue-router';
    import { ref, computed} from 'vue';
    import quizes from '../data/data.json'
    import Result from '../components/Results.vue'

    const route = useRoute()

    // the id of the lessons
    const quizId = parseInt(route.params.id);
    const numberOfCorrectAnswers = ref(0)
    const showResults = ref(false)

    //find specific quiz from quizes data with id similar to quizId
    const quiz = quizes.find(q => q.id === quizId)

    //create a state
    const currentQuestionIndex = ref(0)
    //  listens to any value in the computed callback for changes and rerender it,equal to the below

    const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)

    // const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)
    //
    // watch(() => currentQuestionIndex.value, () => {
    //   questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
    // })
     const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length * 100}%`)
    const onOptionSelected = (isCorrect) => {
        if(isCorrect) {
          numberOfCorrectAnswers.value++;
        }
        if(quiz.questions.length - 1 === currentQuestionIndex.value){
          showResults.value = true
        }
        currentQuestionIndex.value++
    }

</script>


<template>

    <div>
<!--  defining a prop to be used in quizheader     -->
      <QuizHeader
          :questionStatus="questionStatus"
          :barPercentage="barPercentage"
      />
        <div>
          <!-- prop called question equal to quiz -->
            <Question
                v-if="!showResults"
                :question = "quiz.questions[currentQuestionIndex]"
                @selectOption="onOptionSelected"
            />
          <Result
              v-else
          :quizQuestionLength= "quiz.questions.length"
          :numberOfCorrectAnswers="numberOfCorrectAnswers"
          />
        </div>

      <button @click="currentQuestionIndex ++"> Next Question </button>
    </div>
</template>


