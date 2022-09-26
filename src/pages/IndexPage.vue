<template>
  <q-page class="flex flex-center">
    <div>
      <form @submit.prevent="addNewTodo">
        <label>New Todo</label>
        <input v-model="newTodo" name="newTodo" />
        <div class="q-pt-md">
          <q-btn push color="black" text-color="white" label="Add new Todo" />
        </div>
      </form>
      <!--<h2>{{ newTodo }}</h2>-->
      <div>
        <ul>
          <li v-for="todo in todos" :key="todo.id">
            <h3 @click="toggleDone(todo)">{{ todo.content }}</h3>
          </li>
        </ul>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
const newTodo = ref("");
const todos = ref([]);

function addNewTodo() {
  console.log("! - Form succesfully submitted"), console.log(newTodo.value);
  for (let index = 0; index < 100; index++) {
    const element = {
      id: todos.value.length + index,
      title: newTodo.value + " " + todos.value.length,
    };
    todos.value.push(element);
  }

  newTodo.value = "";
}

function toggleDone(todo) {
  todo.done = !todo.cone;
}
</script>
