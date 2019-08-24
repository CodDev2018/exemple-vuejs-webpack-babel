<template>
  <div id="app">
    <div class="container">
      <h1 class="title">Todos</h1>
      <input
        type="text"
        class="input"
        placeholder="What has to be done?"
        v-model="newTodo"
        @keyup="store($event, newTodo)"
      />
      <hr />
      <table class="table is-fullwidth is-narrow">
        <tbody>
          <tr v-for="todo in todos">
            <td>
              <span v-if="!todo.edit">
                <input type="checkbox" v-model="todo.finished" @click="finishe(todo)" />
                <span
                  :class="todo.finished ? 'todo-finished' : ''"
                  @dblclick="edit(todo)"
                >{{todo.description}}</span>
              </span>
              <input
                v-else
                type="text"
                class="input"
                v-model="todo.description"
                @keyup="update($event, todo)"
                @blur="cancelUpdate(todo)"
              />
            </td>
            <td class="has-text-right">
              <button class="button is-small" @click="remove">X</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: null,
      todos: []
    };
  },
  mounted() {
    this.todos = localStorage.todos ? JSON.parse(localStorage.todos) : [];
  },
  methods: {
    store(e, description) {
      if (e.which == 13) {
        this.todos.push({
          description: description,
          finished: false,
          edit: false
        });
        localStorage.todos = JSON.stringify(this.todos);
        this.newTodo = null;
      }
    },

    edit(todo) {
      todo.edit = true;
    },

    update(e, todo) {
      if (e.which == 13) {
        const index = this.todos.indexOf(todo);
        todo.edit = false;
        this.todos[index] = todo;
        localStorage.todos = JSON.stringify(this.todos);
      }
    },

    finishe(todo) {
      const index = this.todos.indexOf(todo);
      todo.finished = !todo.finished;
      this.todos[index] = todo;
      localStorage.todos = JSON.stringify(this.todos);
    },

    cancelUpdate(todo) {
      todo.edit = false;
    },

    remove(todo) {
      const index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
      localStorage.todos = JSON.stringify(this.todos);
    }
  }
};
</script>

<style>
#app {
  width: 600px;
  margin: 10px auto;
}

.todo-finished {
  text-decoration: line-through;
}
</style>