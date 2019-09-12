<template>
  <div>
    <h1>{{ title }}</h1>
    <hr>
    <div class="row">
      <div class="col">
        <form class="form form-inline" @submit.prevent="save">
          <input type="text" class="form-control" placeholder="Nome da Tarefa" v-model="task.name">
          <input type="text" class="form-control" placeholder="Autor da Tarefa" v-model="task.auhor">
          <button class="btn btn-success">Salvar</button>
        </form>
      </div>
      <div class="col">
        <form class="form form-inline">
          <input type="text" class="form-control" placeholder="Filtrar por..." v-model="filter">
        </form>
      </div>
    </div>
    <table class="table table-bordered table-striped table-sm">
      <thead class="thead-dark">
        <tr>
          <th>id</th>
          <th>Nome</th>
          <th>Por</th>
          <th style="width: 180px;">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index ) in taskFiltered" :key="index">
          <td>{{ task.id }}</td>
          <td>{{ task.name }}</td>
          <td>{{ task.auhor }}</td>
          <td>
            <a href="#" class="btn btn-primary btn-sm" @click.prevent="editTask(task.id)">Editar</a>
            <a href="#" class="btn btn-danger btn-sm" @click.prevent="removeTask(task.id)">Excluir</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "Lista de Tarefas",
      tasks: [],
      task: {
        id: "",
        name: "",
        author: ""
      },
      update: false,
      indexToUpdate: "",
      filter: ""
    };
  },
  methods: {
    editTask(id) {
      this.indexToUpdate = this.findIndex(id);
      this.task = this.tasks[this.indexToUpdate];
      this.update = true;
    },
    save() {
      if (this.update == true) {
        this.updateTask();
        this.update = false;
        this.indexToUpdate = "";
        return;
      }

      this.addTask();
    },
    addTask() {
      this.task.id = this.tasks.length + 1;
      this.tasks.push(this.task);
      this.cleanForm();
    },
    updateTask() {
      this.tasks[this.indexToUpdate] = this.task;
      this.cleanForm();
    },
    removeTask(id) {
      this.tasks.splice(this.findIndex(id), 1);
    },
    cleanForm() {
      this.task = {
        id: "",
        name: "",
        author: ""
      };
    },
    findIndex(id) {
      for (let index = 0; index < this.tasks.length; index++) {
        if (this.tasks[index].id === id) return index;
      }
    }
  },
  computed: {
    taskFiltered() {
      if (this.filter === "") return this.tasks;

      let that = this;
      return this.tasks.filter(data => {
        return data.name.toLowerCase().indexOf(that.filter.toLowerCase()) > -1;
      });
    }
  }
};
</script>

<style  scoped>
form {
  margin: 20px 0;
}
input {
  margin: 0 5px;
}
</style>
