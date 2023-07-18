<script setup>
import {ref, watch} from "vue"
const count = ref(0)
const message = ref(' ')
const checked = ref(false)
const checkedNames = ref([])

const question = ref('')
const answer = ref('Questions contain a question mark')


//watch works directly on a ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.indexOf('?') > -1) {
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      anwser.value = (await res.json()).answer
    } catch (err) {
      answer.value = 'Error! Couldnt reach the API' + err
    }
  }
})


</script>


<template>
  <div>
    <p>Message is: {{ message }}</p>
    <input v-model.lazy="message" placeholder="edit me">

    
  <input type="checkbox" id="checkbox" v-model="checked" />
  <label for="checkbox">{{ checked }} </label>

  <div>
    Checked names: {{ checkedNames }}
  </div>
  <input type="checkbox" name="jack" id="jack" v-model="checkedNames" />
  <label for="jack">Jack</label>
  <input type="checkbox" name="john" id="john" v-model="checkedNames" />
  <label for="john">John</label>
  <input type="checkbox" name="mike" id="mike" v-model="checkedNames" />
  <label for="mike">Mike</label>


    <h4>The current count is</h4>
          <h1>{{count}}</h1>
          <button @click="count--">-</button>
          <button @click="count++">+</button>
  </div>

  <div>
    <p>
          Ask a yes/no question:
          <input v-model="question">
      </p>
      <p>
          {{ answer }}
      </p>
  </div>
</template>

<style scoped>
  div > *{
    margin: 10px;
    font-size: 32px;
  }
</style>
