<script setup>
import { reactive, ref } from 'vue'

const emit = defineEmits(['add-question'])

const form = reactive({
  id: '',
  title: '',
  description: '',
  type: 'text',
  placeholder: '',
  options: [],
})

const newOptionLabel = ref('')
const newOptionValue = ref('')

function addOption() {
  if (!newOptionLabel.value || !newOptionValue.value) return
  form.options.push({ label: newOptionLabel.value, value: newOptionValue.value })
  newOptionLabel.value = ''
  newOptionValue.value = ''
}

function removeOption(index) {
  form.options.splice(index, 1)
}

function submit() {
  if (!form.id || !form.title) {
    alert('ID и заголовок обязательны!')
    return
  }

  const question = { ...form }
  if (question.type === 'text') delete question.options
  emit('add-question', question)

  form.id = ''
  form.title = ''
  form.description = ''
  form.type = 'text'
  form.placeholder = ''
  form.options = []
}
</script>

<template>
  <div class="bg-white rounded-xl p-4 shadow mb-4">
    <h2 class="font-semibold mb-4">Создать новый вопрос</h2>

    <div class="mb-2">
      <label class="block font-medium mb-1">ID (обязательно)</label>
      <input v-model="form.id" class="border p-2 w-full rounded" />
    </div>

    <div class="mb-2">
      <label class="block font-medium mb-1">Заголовок вопроса (обязательно)</label>
      <input v-model="form.title" class="border p-2 w-full rounded" />
    </div>

    <div class="mb-2">
      <label class="block font-medium mb-1">Описание (необязательно)</label>
      <input v-model="form.description" class="border p-2 w-full rounded" />
    </div>

    <div class="mb-2">
      <label class="block font-medium mb-1">Тип вопроса</label>
      <select v-model="form.type" class="border p-2 w-full rounded">
        <option value="text">Текст</option>
        <option value="radio">Выбор одного</option>
        <option value="checkbox">Выбор нескольких</option>
        <option value="select">Выпадающий список</option>
      </select>
    </div>

    <div v-if="form.type === 'text'" class="mb-2">
      <label class="block font-medium mb-1">Placeholder</label>
      <input v-model="form.placeholder" class="border p-2 w-full rounded" />
    </div>

    <div v-if="form.type !== 'text'" class="mb-2">
      <h3 class="font-medium mb-1">Варианты ответа</h3>
      <div class="flex gap-2 mb-2">
        <input v-model="newOptionLabel" placeholder="Label" class="border p-2 flex-1 rounded" />
        <input v-model="newOptionValue" placeholder="Value" class="border p-2 flex-1 rounded" />
        <button type="button" class="bg-green-500 text-white px-2 rounded" @click="addOption">
          Добавить
        </button>
      </div>

      <ul>
        <li
          v-for="(opt, i) in form.options"
          :key="i"
          class="flex justify-between items-center border rounded p-2 mb-1"
        >
          <span>{{ opt.label }} ({{ opt.value }})</span>
          <button type="button" class="text-red-500" @click="removeOption(i)">×</button>
        </li>
      </ul>
    </div>

    <button class="bg-blue-600 text-white px-4 py-2 rounded mt-2" @click="submit">
      Добавить вопрос
    </button>
  </div>
</template>
