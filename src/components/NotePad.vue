<script setup>
import Heading from "./Heading.vue";
import { ref, onMounted } from "vue";
const note = ref("");
const limit = ref(20);

const notes = ref([]);

const important = ref(false);

const addNote = () => {
  notes.value.push({
    id: notes.value.length + 1,
    text: note.value,
    important: important.value,
  });
  localStorage.setItem("notes", JSON.stringify(notes.value));
  note.value = "";
  important.value = false;
};

onMounted(() => {
  let data = localStorage.getItem("notes");
  if (data) {
    notes.value = JSON.parse(data);
  } else {
    data = JSON.stringify(notes.value);
  }
});


const removeNote = (id) => {
  const index = notes.value.findIndex((note) => note.id === id);
  notes.value.splice(index, 1);
  localStorage.setItem("notes", JSON.stringify(notes.value));
};



</script>

<template>
  <div class="notepad">
    <Heading />
    <form class="" v-on:submit.prevent="addNote">
      <div class="add-note">
        <input type="text" v-model.trim="note" placeholder="Add note" />
        <button
          type="submit"
          v-bind:disabled="note.length === 0 || note.length > limit"
        >
          +
        </button>
        <p class="limit">
          <span>{{ note.length }}</span> / <span>{{ limit }}</span>
        </p>
      </div>
      <label
        >Important
        <input type="checkbox" v-model="important" />
      </label>
    </form>
    <ul>
      <li
        v-for="{ id, text, important } in notes"
        :key="id"
        :class="{ important: important }"
      >
        {{ text }} <button @click="removeNote(id)">x</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
@import "../assets/base.css";
.notepad {
  background-color: var(--color-6);
  min-width: 300px;
  min-height: 500px;
  border-radius: 10px;
  padding: 1rem;
}
.add-note {
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 1rem;
}
.add-note input {
  color: var(--color-5);
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
}
.add-note button {
  color: var(--color-5);
  border-radius: 50%;
  height: 2rem;
  width: 2rem;
  outline: none;
  border: none;
  font-size: 1.5rem;
  background-color: var(--color-2);
}
.limit {
  position: absolute;
  right: 3rem;
  font-size: 0.8rem;
}
ul {
  padding: 0;
  list-style-type: none;
}
.important {
  border-left: 4px solid var(--color-7);
}
li {
  border-radius: 3px;
  font-size: 1.2rem;
  margin: 1rem 0;
  padding-left: 1rem;
  display: flex;
  justify-content: space-between;
}
li button {
  background-color: rgba(255, 84, 84, 0.915);
  border: none;
  outline: none;
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
  color: var(--color-5);
}
</style>
