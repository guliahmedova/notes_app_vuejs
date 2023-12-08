<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errMsg = ref("");

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length > 10) {
    notes.value.push({
      id: Math.floor(Math.random() * 1000),
      date: new Date(),
      text: newNote.value,
      bgColor: getRandomColor(),
    });
    showModal.value = false;
    newNote.value = "";
    errMsg.value = "";
  } else {
    errMsg.value = "Note needs to be 10 characters or more";
  }
};
</script>

<template>
  <main>
    <div
      class="overlay"
      v-if="showModal"
      @click.prevent.self="showModal = false"
    >
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="notes"
          id="notes"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errMsg" class="text-red">{{ errMsg }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          class="card"
          v-for="item in notes"
          :key="item.id"
          :style="{ backgroundColor: item.bgColor }"
        >
          <p class="main-text">{{ item.text }}</p>
          <p class="date">{{ item.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.container {
  width: 95%;
  padding: 2rem;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 2rem;
  color: whitesmoke;
}
textarea {
  padding: 1rem;
}
header button {
  border: none;
  padding: 1rem;
  width: 2rem;
  height: 2rem;
  cursor: pointer;
  border-radius: 100%;
  background-color: gray;
  font-size: 1.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
.cards-container {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
  margin-top: 3rem;
}
.card {
  min-width: 300px;
  background-color: green;
  padding: 2rem;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  color: black;
  font-weight: 700;
  justify-content: space-between;
  box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
}
.main-text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.date {
  font-size: 1.1rem;
  font-weight: bolder;
  text-align: right;
  margin-top: 2rem;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(128, 128, 128, 0.551);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}
.modal {
  width: 750px;
  background-color: whitesmoke;
  border-radius: 10px;
  padding: 3rem;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button {
  margin-top: 1rem;
  background-color: darkblue;
  color: whitesmoke;
  font-size: 1.2rem;
  cursor: pointer;
  padding: 1rem;
  font-weight: 600;
}
.modal .close {
  background-color: rgb(113, 4, 4);
}
.text-red {
  color: red;
  font-weight: 600;
}
</style>