<template>
  <div class="container" style="width: 500px;">
    <h2 class="text-center mt-5">App Todo List</h2>

        <!-- input -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
    </div>

    <!-- task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Todo</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td class="text-left">
            <span :class="{'complete': task.status === 'complete'}">
              {{task.name}}
            </span>
          </td>
          <td class="text-left" style="width: 120px;">
            <span class="pointer" @click="changeStatus(index)" 
              :class="{
                'text-success': task.status === 'complete',
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress'
              }">
              {{firstCharUpper(task.status)}}
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
            </div></td>
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
  data(){
    return {
      task: '',
      taskEdit: null,
      availableStatus: ['to-do', 'in-progress', 'complete'],

      tasks: [
        {
          name: 'steal your heart',
          status: 'to-do',
        },
        {
          name: 'eat 1kg corn in 1 hour',
          status: 'to-do',
        },
      ]
    }
  },

  methods: {
    submitTask() {
      if(this.task.length === 0) return;
      if(this.taskEdit == null){
          this.tasks.push({
          name: this.task,
          status: 'to-do',
        });
      } else {
        this.tasks[this.taskEdit].name = this.task;
        this.taskEdit = null;
      }
      
      this.task = '';
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.taskEdit = index;
    },

    changeStatus(index){
      let statusIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if(++statusIndex == this.availableStatus.length) statusIndex = 0;
      this.tasks[index].status = this.availableStatus[statusIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer{
  cursor: pointer;
}
.complete{
  text-decoration: line-through;
}
</style>
