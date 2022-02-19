<template>
  <div class="container">
    <h2 class="text-center mt-5">Todo App</h2>

    <!-- Input -->
    <div class="d-flex">
      <input v-model="task" @keyup.enter="submitTask" type="text" placeholder="Enter task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col">#</th>
          <th scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'finished'}">{{task.name}}</span>
            </td>
          <td style="width: 120px">
            <span @click="updateStatus(index)" class="pointer" 
            :class="{'text-danger': task.status === 'to-do',
             'text-warning': task.status === 'in progress',
              'text-success': task.status === 'finished'}">{{firstCharToUpperCase(task.status)}}</span>
          </td>
          <td>
            <div @click="editTask(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <td>
            <div @click="deleteTask(index)" class="text-center">
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
  name: 'TodoApp',
  props: {
    msg: String
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatus: ["to-do", "in progress", "finished"],

      tasks: [
        { name: "Piquer des bananes au super marché.", status: "to-do" },
        { name: "Manger 1kg de chocolat en 1 heure.", status: "in progress" }
      ]
    }
  },

  methods: {
    submitTask() {
      if(this.task.length === 0 ) return;

      if(this.editedTask === null) {
        this.tasks.push({name: this.task, status: "to-do"});
        this.task = "";
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    updateStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
    },

    firstCharToUpperCase(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
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
