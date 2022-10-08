<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue ToDo App</h2>

    <!-- Imput  -->
    <div class="d-flex mt-4">
      <input
        v-model="task"
        class="form-control rounded-3 border border-dark"
        placeholder="Enter Todo List Items" />
    </div>
    <div class="mt-2 text-center">
      <button @click="submitTask" class="btn btn-primary rounded-3">
        Submit
      </button>
    </div>
    <!-- Task Table -->
    <table class="table table-bordered mt-5 border border-dark">
      <thead>
        <tr>
          <th scope="col" class="text-center">Task</th>
          <th scope="col" class="text-center">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td class="text-center">
            <span :class="{ completed: task.Status === 'completed' }">
              {{ task.name }}
            </span>
          </td>
          <td style="width: 120px" class="text-center text-dark">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.Status === 'pending',
                'text-primary': task.Status === 'received',
                'text-secondary': task.Status === 'completed',
              }">
              {{ firstCharUpper(task.Status) }}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen text-info pointer"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash text-danger pointer"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- MOdal popup -->
  </div>
</template>

<script>
  export default {
    name: "HelloWorld",
    props: {
      msg: String,
    },
    data() {
      return {
        task: "",
        editedTask: null,
        avaialableStatus: ["received", "pending", "completed"],
        tasks: [
          {
            name: "Buy Laptop",
            Status: "Pending",
          },
          {
            name: "Buy Mobile",
            Status: "Received",
          },
          {
            name: "Buy Book",
            Status: "Completed",
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
            Status: "Items",
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
        let newIndex = this.avaialableStatus.indexOf(this.tasks[index].Status);
        if (++newIndex > 2) newIndex = 0;
        this.tasks[index].Status = this.avaialableStatus[newIndex];
      },
      firstCharUpper(str) {
        return str.charAt(0).toUpperCase() + str.slice(1);
      },
    },
  };
</script>
<style scopped>
  .pointer {
    cursor: pointer;
  }

  .completed {
    text-decoration: line-through;
    color: black;
    text-decoration-color: black;
  }
</style>
