<template>
  <div class="container">
      <h2>Lista de Tarefas</h2>

      <!-- Inputs -->
      <div class="d-flex">
        <input v-model="task" type="text" class="form-control" placeholder="Digite a tarefa...">
        <button @click="submitTasks" class="btn btn-warning rounded-0">Enviar</button>
      </div>

      <!-- Task Table -->
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
                <span :class="{'finished': task.status === 'finalizado'}">
                  {{task.name}}
                </span></td>
              <td style="width: 120px">
                <span class="pointer" @click="changeStatus(index)"
                :class="{'text-danger': task.status === 'pendente',
                'text-warning': task.status === 'em progresso',
                'text-success': task.status === 'finalizado'
                }">
                  {{firstCharUpper(task.status)}}
                </span>
              </td>
              <td>
                <div class="text-center" @click="editTask(index)">
                    <span class="fa fa-pen pointer"></span>
                </div>
              </td>
              <td>
                <div class="text-center" @click="deleteTask(index)">
                    <span class="fa fa-trash pointer"></span>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
  </div>
</template>

<script>
export default {
  name: 'App',
  props: {
    msg: String
  },
  data() {
    return {
      task: '',
      editedTask: null,
      statuses: ['pendente', 'em progresso', 'finalizado'],
      tasks: [{
        name: 'Hospedar portfolio.',
        status: 'pendente'
      },
      {
        name: 'Comer chocolates.',
        status: 'em progresso'
      }
      ]
    }
  },
  methods:{
    submitTasks() {
      if(this.task.length === 0) return;

      if(this.editedTask === null) {
      this.tasks.push({
        name: this.task,
        status: 'Pendente'
      })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }
      
      this.task = ''
    },
    deleteTask(index) {
    this.tasks.splice(index, 1)
   },
      editTask(index) {
     this.task = this.tasks[index].name
     this.editedTask = index
   },
   changeStatus(index) {
    let newStatus = this.statuses.indexOf(this.tasks[index].status)
    if(++newStatus > 2) newStatus = 0
     this.tasks[index].status = this.statuses[newStatus]
   },
   firstCharUpper(string) {
     return string.charAt(0).toUpperCase() + string.slice(1)
   }
 }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
