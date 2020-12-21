<template>
  <b-container class="bv-example-row mt-4">
    <b-row style="margin-top: 34px">
      <b-col md="6">
        <b-card-header header-bg-variant="danger" header-text-variant="white">
          <b-icon icon="list-task" />
          <div style="float: right">A fazer</div>
        </b-card-header>
        <b-card
          class="shadow-lg mb-4"
          style="border: none; border-radius: 0px 0px 5px 5px"
        >
          <b-alert
            v-if="todoList.undone == 0"
            class="mt-3"
            show
            variant="warning"
          >
            Você não possui nenhuma tarefa a <b>fazer</b>.
          </b-alert>
          <b-list-group v-else>
            <b-card
              bg-variant="light"
              v-for="(todo, index) in todoList.undone"
              :header="todo.name"
              :key="index"
              class="mt-3 border text-left shadow-sm animate__animated animate__fadeInRight"
            >
              {{ todo.task }}
              <br />
              <br />
              <b-button
                @click="checkTodo(todo, index)"
                size="sm"
                class="float-right ml-2"
                variant="success"
              >
                <b-icon icon="check" />
              </b-button>
              <b-button
                @click="deleteTodo(todo, index)"
                size="sm"
                class="float-right"
                variant="secondary"
              >
                <b-icon icon="trash-fill" />
              </b-button>
            </b-card>
          </b-list-group>
        </b-card>
      </b-col>
      <b-col md="6">
        <b-card-header header-bg-variant="success" header-text-variant="white">
          <b-icon icon="list-check" />
          <div style="float: right">Concluídas</div>
        </b-card-header>
        <b-card
          class="shadow-lg mb-4"
          style="border: none; border-radius: 0px 0px 5px 5px"
        >
          <b-alert
            v-if="todoList.done == 0"
            class="mt-3"
            show
            variant="warning"
          >
            Você não possui nenhuma tarefa <b>concluída</b>.
          </b-alert>
          <b-list-group v-else>
            <b-card
              bg-variant="light"
              v-for="(todo, index) in todoList.done"
              :header="todo.name"
              :key="index"
              class="mt-3 border text-left shadow-sm animate__animated animate__fadeInLeft"
            >
              {{ todo.task }}
              <br />
              <br />
              <b-button
                @click="uncheckTodo(todo, index)"
                size="sm"
                class="float-right ml-2"
                variant="danger"
              >
                <b-icon icon="arrow-counterclockwise" />
              </b-button>
              <b-button
                @click="deleteTodo(todo, index)"
                size="sm"
                class="float-right"
                variant="secondary"
              >
                <b-icon icon="trash-fill" />
              </b-button>
            </b-card>
          </b-list-group>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from "axios";

export default {
  props: {
    todoList: { type: Object, required: true },
  },
  data() {
    return {
      animation: "left",
    };
  },
  mounted() {
    axios.get("http://localhost:8080/todos").then((res) => {
      console.log(res.data);
      this.todoList.undone = res.data;
    });
    axios.get("http://localhost:8080/todos?done=true").then((res) => {
      console.log(res.data);
      this.todoList.done = res.data;
    });
  },
  methods: {
    checkTodo(todo, index) {
      this.todoList.done.push(JSON.parse(JSON.stringify(todo)));
      this.todoList.undone.splice(index, 1);
      axios.put(`http://localhost:8080/todos/${todo.id}`, { done: true });
    },
    uncheckTodo(todo, index) {
      this.todoList.undone.push(JSON.parse(JSON.stringify(todo)));
      this.todoList.done.splice(index, 1);
      axios.put(`http://localhost:8080/todos/${todo.id}`, { done: false });
    },
    async deleteTodo(todo) {
      await axios.delete(`http://localhost:8080/todos/${todo.id}`);
      await axios.get("http://localhost:8080/todos").then((res) => {
        this.todoList.undone = res.data;
        this.$forceUpdate();
      });
      await axios.get("http://localhost:8080/todos?done=true").then((res) => {
        this.todoList.done = res.data;
        this.$forceUpdate();
      });
    },
  },
};
</script>