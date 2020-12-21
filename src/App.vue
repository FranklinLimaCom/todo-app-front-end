<template>
  <div id="app">
    <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
      <h1 class="display-4 text-white">To<b class="font-weight-bold">Do</b></h1>
      <p class="lead text-white">
        Feito por <b>Franklin Lima</b> para a empresa <b>SANP Tecnologia</b>.
      </p>
    </div>
    <TodoForm :todo-list="todos" />
    <TodoList :todo-list="todos" />
  </div>
</template>

<script>
import TodoForm from "./components/TodoForm";
import TodoList from "./components/TodoList";

export default {
  name: "App",
  components: {
    TodoForm,
    TodoList,
  },
  data() {
    return {
      todos: {
        undone: [],
        done: [],
      },
    };
  },
};
</script>

<style>
  body {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    width: 100%;
    min-height: 100vh !important;
    background-image: linear-gradient(
        330deg,
        rgba(225, 225, 225, 0.05) 0%,
        rgba(225, 225, 225, 0.05) 33.333%,
        rgba(114, 114, 114, 0.05) 33.333%,
        rgba(114, 114, 114, 0.05) 66.666%,
        rgba(52, 52, 52, 0.05) 66.666%,
        rgba(52, 52, 52, 0.05) 99.999%
      ),
      linear-gradient(
        66deg,
        rgba(181, 181, 181, 0.05) 0%,
        rgba(181, 181, 181, 0.05) 33.333%,
        rgba(27, 27, 27, 0.05) 33.333%,
        rgba(27, 27, 27, 0.05) 66.666%,
        rgba(251, 251, 251, 0.05) 66.666%,
        rgba(251, 251, 251, 0.05) 99.999%
      ),
      linear-gradient(
        225deg,
        rgba(98, 98, 98, 0.05) 0%,
        rgba(98, 98, 98, 0.05) 33.333%,
        rgba(222, 222, 222, 0.05) 33.333%,
        rgba(222, 222, 222, 0.05) 66.666%,
        rgba(228, 228, 228, 0.05) 66.666%,
        rgba(228, 228, 228, 0.05) 99.999%
      ),
      linear-gradient(90deg, rgb(28, 20, 63), rgb(40, 160, 253));
  }
</style>
