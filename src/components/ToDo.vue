<template>
  <div class="container-fluid">
    <h1 class="text-center mt-5">My Vue Todo App</h1>

    <div class="d-flex mt-5 ">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-warning">
        SUBMIT
      </button>
    </div>

    <table class="table table-bordered mt-5">
      <thead class="thead-dark">
        <tr>
          <th >Task</th>
          <th >Status</th>
          <th  class="text-center">#</th>
          <th  class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td style="width: 500px;">
            <span :class="{'finished': task.status === 'finished'}">
              {{ firstCharUpper(task.name) }}
            </span>
            </td>
          <td style="width: 120px;">
            <span @click="changeStatus(index)" class="pointer" :class="{'text-danger': task.status === 'to-do', 
            'text-warning': task.status === 'in-progress',
            'text-success': task.status === 'finished'}">
              {{ firstCharUpper(task.status) }}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "ToDo",
  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "take banana",
          status: "to-do",
        }
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
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
      let newStatus = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newStatus > 2) newStatus = 0;
      this.tasks[index].status = this.availableStatuses[newStatus];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  },
};
</script>

<style scoped>

.container-fluid {
  margin-top: 120px;
  width: 60%;
}

.thead-dark {
  background-color: black;
  color: white;
}

.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
