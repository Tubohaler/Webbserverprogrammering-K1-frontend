<script setup>
import { ref } from "vue";
import axios from "axios";

const todoEmpty = ref("");
const taskList = ref([]);
const deleteTodo = ref([]);

const getTodos = async () => {
  axios.get("http://localhost:4000/todos").then(({ data }) => {
    taskList.value = data;
  });
};

const addTodos = async () => {
  axios
    .post("http://localhost:4000/todos", {
      name: todoEmpty.value,
      done: false,
    })
    .then(() => getTodos());
  todoEmpty.value = "";
};

function deleteActivity(id) {
  axios.delete(`http://localhost:4000/todos/${id}`).then(() => getTodos());
}

function completedTodos(target) {}

getTodos();
</script>

<template>
  <h1 class="background">David's Todo-Shite!</h1>

  <label>Do something!</label>
  <div class="todo-container">
    <input class="form-input" name="newTodo" v-model="todoEmpty" />
    <button class="complete-btn" @click="addTodos">Add</button>
  </div>

  <div class="list">
    <div v-for="(todo, index) in taskList" class="listItem" :key="todo.id">
      <span :class="{ completed: todo.done }">{{ todo.name }} </span>
      <input
        @click="completedTodos(index)"
        type="checkbox"
        :checked="todo.done"
        v-model="todo.done"
      />
      <button class="trash-btn" @click="deleteActivity(todo.id)">Kill</button>
    </div>
  </div>
</template>

<style>
.background {
  color: gold;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  font-family: fantasy;
  font-size: xxx-large;
  margin: -1rem;
  z-index: 0;
}

.backgroundImg {
  height: 75vh;
  position: absolute;
  opacity: 0.1;
  margin-top: -2rem;
}

label {
  margin-left: 0px;
  font-size: xx-large;
  font-family: cursive;
  letter-spacing: 0.3rem;
  color: goldenrod;
}

.list {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.listItem {
  height: 25px;
  border: 3px solid rgb(255, 255, 255);
  margin: 0.5rem;
  background: white;
  font-size: 1.5rem;
  color: black;
  display: flex;
  justify-content: space-between;
  align-items: center;
  min-width: 31.9%;
  list-style: none;
  margin-left: 3rem;
  margin-top: 0.7rem;
  z-index: 3;
}

input {
  padding: 0px;
}

button {
  border: 0px;
  background-color: orange;
  margin: 0rem;
}

.form-input,
form button {
  font-size: 2.73rem;
  border: none;
  margin-bottom: 0.5rem;
  z-index: 2;
}
form button {
  color: #ff6f47;
  background: #f7fffe;
  cursor: pointer;
  transition: all 0.3s ease;
}

.trash-btn,
.complete-btn {
  background: #ff6f47;
  color: white;
  border: none;
  margin-bottom: 0.5rem;
  padding: 1rem;
  cursor: pointer;
  font-size: 1rem;
}

.complete-btn {
  background: rgb(11, 212, 162);
}

@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.12.1/css/all.min.css ");

.completed {
  text-decoration: line-through;
}

.todo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 2vw;
}

.todo {
  margin: 0.5rem;
  background: white;
  font-size: 1.5rem;
  color: black;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 1s ease;
  width: 70%;
  min-width: 30%;
  list-style: none;
  z-index: 3;
}
</style>