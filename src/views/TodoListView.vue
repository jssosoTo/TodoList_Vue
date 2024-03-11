<script setup>
import { ref, reactive } from 'vue'

const inputValue = ref('')
const todoList = ref([])
const editInfo = reactive({
  isEdit: false,
  editIdx: null
})

async function addList() {
  if (!inputValue.value) return alert('A todoList must be more or equal than 1 character')

  if (!editInfo.isEdit) {
    todoList.value.push(inputValue.value)
    inputValue.value = ''
  } else {
    todoList.value.splice(editInfo.editIdx, 1, inputValue.value)
    inputValue.value = ''
    editInfo.isEdit = false
    editInfo.editIdx = null
  }
}

function removeList(i) {
  todoList.value.splice(i, 1)
}

function editList(i) {
  inputValue.value = todoList.value.find((_, idx) => idx === i)
  editInfo.isEdit = true
  editInfo.editIdx = i
}

const h2Style = {
  textAlign: 'center',
  textShadow: '0 0 2px rgba(0, 0, 0, .4)',
  backgroundColor: 'black',
  color: 'gray',
  padding: '.2rem'
}
</script>

<template>
  <div class="bar">
    <h2 :style="h2Style">Todo List</h2>
    <main class="Input">
      <section class="inputContainer">
        <input
          placeholder="e.g egg"
          :value="inputValue"
          @input="(e) => (inputValue = e.target.value)"
        />
        <button v-on:click="addList">{{ editInfo.isEdit ? 'Edit' : 'Add' }}</button>
      </section>
      <section class="todoList">
        <ul>
          <li class="list" v-for="(item, idx) of todoList" :key="idx">
            <h4>{{ item }}</h4>
            <div :style="{ display: 'flex', gap: '.4rem' }">
              <button @click="editList(idx)">
                {{ editInfo.editIdx === idx ? 'Editing' : 'Edit' }}
              </button>
              <button @click="removeList(idx)">Remove</button>
            </div>
          </li>
        </ul>
      </section>
    </main>
  </div>
</template>

<style scoped>
.bar {
  overflow: hidden;
  border-radius: 0.5rem;
  box-shadow: 0 0 20px 2px rgba(0, 0, 0, 0.2);

  width: 550px;
  height: 60vh;
  margin: auto;

  display: flex;
  flex-direction: column;
}

main {
  display: flex;
  flex-direction: column;

  overflow: auto;
}

.Input {
  padding: 0.5rem;
}

.inputContainer {
  display: flex;
  justify-content: space-between;

  gap: 0.8rem;

  padding: 0 2rem;
}

.inputContainer input {
  flex: 1;
  height: 2rem;

  border-radius: 5px;
  padding: 0.5rem;
  border: 2px solid black;
}

.inputContainer button {
  cursor: pointer;
  border: none;
  background-color: transparent;
  border-radius: 5px;
  padding: 0 1rem;

  background-color: black;
  color: white;
  transition: all 0.3s;
}

.inputContainer button:hover {
  background-color: rgb(24, 24, 24);
}

.todoList {
  flex: 1;
  overflow: auto;
  padding: 0.5rem;
}

.todoList ul {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
}

.list {
  display: flex;
  justify-content: space-between;
}

.list h4 {
  flex: 1;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.list button {
  cursor: pointer;
  border: none;
  background-color: transparent;
  border-radius: 5px;
  padding: 0 0.5rem;

  background-color: black;
  color: white;
  transition: all 0.3s;
}

.list button:hover {
  background-color: rgb(24, 24, 24);
}
</style>
