<template>
  <q-page class="flex flex-center background">
    <div>
      <div class="row q-pa-sm flex flex-center">
        <q-bar
          dense
          style="
            background-color: rgba(0, 0, 0, 0.9);
            border-radius: 20px;
            color: orange;
          "
        >
          <div>ToDo App</div>
          <q-icon name="menu_open" />
          <q-space />
          <q-icon name="bluetooth" />
          <q-icon name="signal_wifi_4_bar" />
          <q-icon name="signal_cellular_4_bar" />
          <div class="gt-xs">100%</div>
          <q-icon name="battery_full" />
          <div>10:00AM</div>
        </q-bar>
      </div>

      <div class="row flex flex-center q-pa-sm">
        <form @submit.prevent="addNewTodo" style="width: 500px; height: 75px">
          <q-input standout bottom-slots dark v-model="newTodo">
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
        style="background-color: rgba(0, 0, 0, 0.9); border-radius: 20px"
      >
        <q-scroll-area
          dark
          style="width: 500px; height: calc(100vh - 250px)"
          class="q-pt-md flex flex-center"
        >
          <div v-if="todos.length > 0">
            <div
              v-for="(todo, index) in todos"
              dark
              :key="index"
              class="q-ma-md my-card"
            >
              <!-- <q-separator color="orange" inset /> -->
              <div class="row flex flex-center">
                <div class="col-6 text-white q-pa-md flex">
                  {{ todo.content }}
                </div>
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
            <q-separator color="orange" inset />
            <div style="height: 5px; width: max-content" />
            <q-separator color="orange" inset />
          </div>
          <div v-else class="flex flex-center text-white">en exei</div>
        </q-scroll-area>
      </div>
    </div>
    <q-dialog v-model="editToDo">
      <q-card>
        <div>
          <div
            class="row text-color:white"
            style="background-color: rgba(0, 0, 0, 0.9)"
          >
            <q-input v-model="editToDoInput" type="text" />
          </div>
          <div class="row">
            <q-btn
              color="orange"
              icon="check"
              label="Save"
              no-caps
              @click="saveEditedToDo"
            />
          </div>
        </div>
      </q-card>
    </q-dialog>
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
const editToDo = ref(false);
const editToDoInput = ref("");
const editingToDoIndex = ref(0);

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
  editToDo.value = true;
  editingToDoIndex.value = index;
}

function updateTodoItem() {
  LocalStorage.set("toDoList", todos.value);
}

function saveEditedToDo() {
  todos.value[editingToDoIndex.value].content = editToDoInput;
  editToDo.value = false;
  editingToDoIndex.value = 0;
  LocalStorage.set("toDoList", todos.value);
}
</script>
