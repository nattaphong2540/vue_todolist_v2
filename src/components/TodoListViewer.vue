<template>
    <b-list-group>
        <b-list-group-item class="d-flex justify-content-between align-items-center" v-for="(todo, index) in todolist" :key="index">
            <div>
                <input
                    type="checkbox"
                    name="todo"
                    v-model="todo.completed"
                    @change="updateCompleted(todo.id, todo.completed)"
                />
                {{ todo.title }}
            </div>
            <div>
                <b-button pill variant="danger" @click="removeTodo(todo.id)">x</b-button>
            </div>
        </b-list-group-item>
    </b-list-group>
</template>

<script>
export default {
  name: "todo-list-viewer",
  props: {
    todolist: Array,
  },
  components: {},
  created() {},
  data: function () {
    return {};
  },
  methods: {
    removeTodo(id) {
      this.$emit("removeTodo", id);
    },
    updateCompleted(id, completed) {
      this.$emit("updateCompleted", id, completed);
    },
  },
};
</script>