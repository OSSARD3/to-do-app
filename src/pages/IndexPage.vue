<template>
  <q-page class="flex flex-center background">
    <div>
      <div class="row q-pa-sm flex flex-center">
        <q-banner
          rounded
          class="text-orange"
          style="background-color: rgba(0, 13, 19, 0.82); width: 500px"
        >
          TODO LIST
        </q-banner>
      </div>
      <div class="row flex flex-center q-pa-sm">
        <form @submit.prevent="addNewTodo" style="width: 500px; height: 75px">
          <q-input outlined bottom-slots dark v-model="newTodo">
            <template v-slot:append>
              <q-btn
                round
                dense
                flat
                icon="add"
                type="submit"
                @click="addNewTodo"
              />
            </template>
          </q-input>
        </form>
      </div>
      <div
        class="row q-pa-sm flex flex-center"
        style="background-color: rgba(0, 13, 19, 0.82); border-radius: 20px"
      >
        <q-scroll-area
          dark
          style="width: 500px; height: calc(100vh - 250px)"
          class="q-pt-md"
        >
          <div v-if="todos.length > 0">
            <div
              v-for="(todo, index) in todos"
              dark
              :key="index"
              class="q-ma-md"
              style="width: 450px; height: 100px"
            >
              <div class="row flex flex-center">
                <div class="col-6 text-white">{{ todo.content }}</div>
                <div class="col-2">
                  <q-checkbox
                    dark
                    v-model="todo.done"
                    checked-icon="task_alt"
                    color="orange"
                    unchecked-icon="panorama_fish_eye"
                    @click="updateTodoItem"
                  />
                </div>
                <div class="col-2 text-white">
                  <q-btn
                    rounded
                    icon="close"
                    size="sm"
                    @click="deleteTodoItem(index)"
                  />
                </div>
                <div class="col-2 text-white">
                  <q-btn
                    rounded
                    icon="edit_note"
                    size="sm"
                    @click="editTodoItem(index)"
                  />
                </div>
              </div>
            </div>
          </div>
          <div v-else class="flex flex-center text-white">en exei</div>
        </q-scroll-area>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { LocalStorage } from "quasar";

const newTodo = ref("");
const todos = ref(
  LocalStorage.getItem("toDoList") !== null
    ? LocalStorage.getItem("toDoList")
    : []
);

function addNewTodo() {
  console.log("! - Form succesfully submitted"), console.log(newTodo.value);
  const item = {
    content: newTodo.value,
    done: false,
  };
  todos.value.push(item);
  LocalStorage.set("toDoList", todos.value);
  newTodo.value = "";
}

function deleteTodoItem(index) {
  console.log("mphke ston kwlo mou");
  todos.value.splice(index, 1);
  LocalStorage.set("toDoList", todos.value);
}
function editTodoItem(index) {
  console.log("to kanei edit gamw");
  elementIndex = toDolist.findIndex((obj) => obj.id == 1);
  console.log("Before update: ", toDolist[elementIndex]);
  toDolist[elementIndex].name = "Ubuntu";
  console.log("After update: ", toDolist[elementIndex]);
  const element = LocalStorage.set("toDoList", todos.value);
}

function updateTodoItem() {
  LocalStorage.set("toDoList", todos.value);
}
</script>
