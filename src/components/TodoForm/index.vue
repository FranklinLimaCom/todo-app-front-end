<template>
  <b-container class="bv-example-row">
    <b-row>
      <b-col class="mt-2">
        <b-card style="border: none" class="shadow-lg">
          <b-form-input
            v-model="todo.name"
            placeholder="Nome da tarefa"
          ></b-form-input>
          <b-form-textarea
            id="textarea"
            v-model="todo.task"
            class="mt-2"
            placeholder="Detalhes da tarefa"
            rows="3"
            max-rows="6"
          />
          <b-button
            block
            class="mt-2"
            :variant="disabled ? 'outline-secondary' : 'outline-primary'"
            @click="newTodo"
            :disabled="disabled"
            >Adicionar Tarefa
          </b-button>
          <div v-if="errorMsg.length > 0" class="mt-3 ml-1" style="color: #555">
            * {{ errorMsg }}
          </div>
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
      todo: { name: "", task: "" },
      errorMsg: "Você precisa dar um nome à tarefa.",
      disabled: true,
    };
  },

  watch: {
    todo: {
      deep: true,
      handler() {
        if (this.todo.name.length == 0) {
          this.errorMsg = "Você precisa dar um nome à tarefa.";
          this.disabled = true;
        } else if (this.todo.task.length == 0) {
          this.errorMsg = "Você precisa dar uma descrição à tarefa.";
          this.disabled = true;
        } else if (this.todo.task.length > 300) {
          this.errorMsg =
            "A descrição da tarefa não pode ter mais de 300 caracteres.";
          this.disabled = true;
        } else if (this.todo.name.length > 200) {
          this.errorMsg =
            "O nome da tarefa não pode ter mais de 200 caracteres.";
          this.disabled = true;
        } else {
          this.disabled = false;
          this.errorMsg = "";
        }
      },
    },
  },

  methods: {
    newTodo() {
      this.todoList.undone.push(JSON.parse(JSON.stringify(this.todo)));
      axios.post("http://localhost:8080/todos", this.todo);
      this.todo = { name: "", task: "" };
    },
  },
};
</script>