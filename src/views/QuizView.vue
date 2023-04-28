<script setup>
    import Question from "../components/Question.vue"
    import QuizHeader from "../components/QuizHeader.vue"
    import Result from "../components/Result.vue"
    import {useRoute} from "vue-router"
    import quizes from "../data/quizes.json"
    import { ref, watch, computed } from "vue"

    const route = useRoute()
    const quizId = parseInt(route.params.id)

    // 在 quizes 中找里面的 item.id === quizId 返回符合条件的第一个元素
    const quiz = quizes.find(q => q.id === quizId)
    const currentQuestionIndex = ref(0)
    const numberOfCorrectAnswers = ref(0)
    const showResults = ref(false)

    // const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)
    // // 第一个参数是一个回调函数，返回的值是想要监听的元素
    // // 第二个参数也是一个回调函数，当被监听的元素改变时就执行
    // watch(() => currentQuestionIndex.value, () => {
    //     questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
    // })

    // 当被计算的元素变化时，就会重新计算，然后重新渲染页面中用到的这些元素
    const questionStatus = computed(() => { `${currentQuestionIndex.value}/${quiz.questions.length}`})
    const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length * 100}%`)
    
    const onOptionSelected = (isCorrect) => {
        if(isCorrect) {
            numberOfCorrectAnswers.value++;
        }

        if(quiz.questions.length - 1 === currentQuestionIndex.value) {
            showResults.value = true
        }
        currentQuestionIndex.value++
    }
</script>

<template>
    <div>
        <QuizHeader 
        :questionStatus="questionStatus"
        :barPercentage="barPercentage" />
        <div>
            <Question
                v-if="!showResults"
                :question="quiz.questions[currentQuestionIndex]" 
                @selectOption = "onOptionSelected"
            />
            <Result 
                v-else
                :quizQuestionLength="quiz.questions.length"
                :numberOfCorrectAnswers="numberOfCorrectAnswers"
            />
        </div>
    </div>
</template>

<style scoped>


</style>