<template>
  <div class="container">
    <h2 class="text-center mt-5">Meu aplicativo Vue Todo</h2>

    <!-- INPUT -->
    <div class="d-flex mt-5">
      <input
        v-model="task"
        type="text"
        class="form-control"
        placeholder="Digite a tarefa"
        name=""
        id=""
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        Enviar
      </button>
    </div>

    <!-- Tarefa table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Tarefa</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ Finalizado : task.status === 'Finalizado ' }">{{
              task.name
            }}</span>
          </td>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-success': task.status === 'A Ser Feito',
                'text-warning': task.status === 'Em Progresso',
                'text-danger': task.status === 'Finalizado  ',
              }"
              >{{ firstCharUpper(task.status) }}</span
            >
          </td>
          <td class="text-center" @click="editTask(index)">
            <div>
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td class="text-center" @click="deleteTask(index)">
            <div><span class="fa fa-trash"></span></div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["A Ser Feito", "Em Progresso", "Finalizado "],
      tasks: [
        {
          name: "Pegar Crianças na Escola.",
          status: "A Ser Feito",
        },
        {
          name: "Fazer Compras no Mercado",
          status: "Em Progresso",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "A Ser Feito",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}

.Finalizado  {
  text-decoration: line-through;
}
</style>