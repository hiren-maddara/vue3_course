
<script setup>
import {ref, watch} from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMessage = ref('')

function closeModal(e){
  if (e.target.classList.contains('overlay')) showModal.value = false
}



function getRandomColor(){
  return `hsl(${Math.random() * 360}, 100%, 75%)`
}

const addNote = () => {
  if (newNote.value.length > 10){
    notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  })

  newNote.value = ''
  showModal.value = false
  errorMessage.value = null
}
}

watch(newNote, () => {
  console.log(newNote.value.length)
  if (newNote.value.length === 0 || newNote.value.length > 10) errorMessage.value = null
  else errorMessage.value = "Note should be 10 characters or more"

})

</script>

<template>
  <main>
    <div v-if="showModal" class="overlay" @click="closeModal">
      <div class="modal" @keyup.ctrl.enter="addNote">
        <textarea name="note" id="note" cols="30" rows="10" placeholder="Write here" v-model.trim="newNote" @change="newNote.length < 10 ? (errorMessage = 'Note should be 1o xters or more') : null">{{ newNote }}</textarea>
        <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
        <button id="btnAddNote" @click="addNote">Add Note</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = !showModal">+</button>
      </header>

      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="note.id" :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date }}</p>
        </div>
        
      </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width: 100vw;
}

.container{
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgba(21,20, 20);
  border-radius: 100%;
  color: white;
  font-size: 25px;
}

.card {
  width: 225px;
  max-width: 225px;
  max-height: 255px;
  aspect-ratio: 1;
  background-color: rgba(237, 182, 44);
  padding: 10px;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  overflow: hidden;
}

.card .main-text {
  color: rgb(26, 25, 25);
  font-weight: 800;

}
.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,.77);
  z-index: 20;
  display: grid;
  place-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 15px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal .error{
  color: red;
}
.modal button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal textarea {
  border-radius: 10px;
  padding: 15px;
  font-size: 20px;
  border: 1px dotted blueviolet;
}

.modal textarea:active, .modal textarea:focus-visible {
  border-radius: 10px;
  border: 1px solid blueviolet;
}

.modal textarea::placeholder{
  font-weight: lighter;
}
</style>