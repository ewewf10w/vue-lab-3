<script setup>
import { ref } from 'vue'
import SurveyForm from './components/SurveyForm.vue'
import SummaryPanel from './components/SummaryPanel.vue'
import { surveyQuestions as initialQuestions } from './data/questions.js'
import QuestionCreator from './components/QuestionCreator.vue'

const answers = ref({})
const isFinished = ref(false)
const surveyFormRef = ref(null)
const surveyQuestions = ref([...initialQuestions])

function onFinish() {
  isFinished.value = true
}

function onAnswer({ id, value }) {
  answers.value[id] = value
}
function resetSurvey() {
  answers.value = {}
  isFinished.value = false
  surveyFormRef.value?.resetStep()
}

function addQuestion(question) {
  if (surveyQuestions.value.find((q) => q.id === question.id)) {
    alert('Вопрос с таким ID уже существует!')
    return
  }
  surveyQuestions.value.push(question)
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 text-gray-900">
    <div class="max-w-6xl mx-auto p-6">
      <h1 class="text-3xl font-bold mb-6">Опрос о путешествиях</h1>

      <div class="grid gap-6 md:grid-cols-2">
        <div class="bg-white rounded-2xl shadow p-6">
          <SurveyForm
            ref="surveyFormRef"
            :questions="surveyQuestions"
            :answers="answers"
            @answer="onAnswer"
            @finish="onFinish"
          />
        </div>

        <SummaryPanel
          v-if="isFinished"
          :answers="answers"
          :survey-questions="surveyQuestions"
          @reset="resetSurvey"
        />
      </div>
      <div class="mx-auto mt-6 w-full max-w-md">
        <QuestionCreator @add-question="addQuestion" />
      </div>
    </div>
  </div>
</template>
