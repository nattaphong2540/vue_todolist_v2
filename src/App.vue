<template>
  <div id="app">
    <b-row>
      <b-col cols="12">
        <b-row class="justify-content-md-center">
          <b-col cols="12" class="text-center">
            <h1>TODO LIST V2</h1>
          </b-col>
        </b-row>
      </b-col>
      <b-col cols="12">
        <b-row class="justify-content-md-center">
          <b-col cols="8">
            <todo-list-viewer
              ref="todo-component"
              :todolist="todolist"
              @removeTodo="removeTodo"
              @updateCompleted="updateCompleted"
            />
          </b-col>
        </b-row>
      </b-col>
      <b-col cols="12">
        <b-row class="justify-content-md-center">
          <b-col cols="8">
           <b-input-group prepend="title" class="mt-3">
              <b-form-input v-model="title"></b-form-input>
              <b-input-group-append>
                <b-button variant="success" @click="addTodo"
                  >Add</b-button
                >
              </b-input-group-append>
            </b-input-group>
          </b-col>
        </b-row>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from "axios";
import TodoListViewer from "./components/TodoListViewer";

export default {
  name: 'App',
  components: {
    TodoListViewer,
  },
  created() {
    this.loadTodo();
  },
  data: function () {
    return {
      title: "",
      todolist: [],
    };
  },
  methods: {
    loadTodo() {
      axios
        .get("https://arcane-hollows-66380.herokuapp.com/todos")
        .then(({ data }) => {
          this.todolist = data;
        });
    },
    addTodo() {
      const data = {
        username: "Thomas",
        title: this.title,
      };
      axios
        .post(`https://arcane-hollows-66380.herokuapp.com/todos`, data)
        .then(() => {
          this.loadTodo();
        });
      this.title = "";
    },
    removeTodo(id) {
      axios
        .delete(`https://arcane-hollows-66380.herokuapp.com/todos/${id}`)
        .then(() => {
          this.loadTodo();
        });
    },
    updateCompleted(id, completed) {
      let updateData = {
        completed,
      };
      axios
        .put(
          `https://arcane-hollows-66380.herokuapp.com/todos/${id}`,
          updateData
        )
        .then(() => {
          this.loadTodo();
        });
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
