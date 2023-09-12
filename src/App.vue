<script setup>
  import {ref} from "vue"

  const showModal = ref(false)
  const newNote = ref("")
  const notes = ref([])
  const errorMessage = ref("")

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
  if (newNote.value.length < 10) {
    return errorMessage.value = "Note needs to be 10 characters or more."
  }
    notes.value.push({
      id: Math.floor(Math.random() * 100000),
      text: newNote.value,
      color: getRandomColor(),
      date: new Date()
    })
    showModal.value = false;
    newNote.value = ""
    errorMessage.value = ""
  }
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <p class="close" @click="showModal = false">x</p>
        <textarea v-model.trim="newNote"/>
        <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" class="card" :style="{backgroundColor: note.color}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date" style="text-align: end">{{ note.date.toLocaleDateString("fr-FR") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.error {
  padding: 5px 0;
  color: rgb(193,15,15);
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  color: #222222;
}

.main-text {
  line-height: 1.25;
  font-size: 17.5px;
  font-weight: bold;
}

.date {
  font-size: 12.5px;
  margin-top: auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 1000px;
  font-size: 20px;
  transition: 0.5s;
}

header button:hover {
  background-color: #b28ee9;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

main {
  height: 100vh;
  width: 100vw;
}

.modal {
  width: 750px;
  background-color: #222222;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  border: none;
  cursor: pointer;
  margin-top: 15px;
  border-radius: 3px;
  transition: 0.5s;
  background-color: #b28ee9;
}

.modal button:hover {
  background-color: buttonface;
}

.modal p {
  margin-left: auto;
  font-size: 20px;
  z-index: 100000;
  cursor: pointer;
}

textarea {
  width: 100%;
  height: 200px;
  padding: 5px;
  font-size: 20px;
  border: none;
  border-radius: 3px;
}

.close {
  font-weight: bold;
  padding: 0 10px 10px 10px;
}
</style>
