<template>
  <q-page class="flex justify-center page">
    <q-card class="content">
      <div class="row q-pa-sm bg-primary">
        <q-input class="col" bg-color="white" @keyup.enter="addTask" v-model="newTask" placeholder="Add Task" filled
          dense>
          <template v-slot:append>
            <q-btn @click="addTask" round dense flat icon="add" />
          </template>
        </q-input>
      </div>
      <q-list separator bordered>
        <q-item v-for="(task, i) in tasks" :key="task.id" :class="{ 'task-done': task.done }"
          @click="task.done = !task.done" clickable v-ripple>
          <q-item-section avatar>
            <q-checkbox v-model="task.done" class="no-pointer-events" color="primary" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ task.title }}</q-item-label>
          </q-item-section>
          <q-btn @click.stop="deleteTask(i)" flat round dense color="primary" icon="delete" />
        </q-item>
      </q-list>
      <div v-if="!tasks.length" class="no-tasks absolute-center">
        <q-icon name="check" size="100px" color="primary"></q-icon>
        <div class="text-h5 text-primary text-center">No Tasks</div>
      </div>
    </q-card>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "HomePage",
  data() {
    return {
      newTask: "",
      tasks: [
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure you want to delete this task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify({
          message: `Task ${index + 1} deleted`,
          color: 'primary',
        })
      }).onCancel(() => {
        console.log('>>>> Cancel')
      }).onDismiss(() => {
        console.log('I am triggered on both OK and Cancel')
      })
    },

    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({
          id: this.tasks.length + 1,
          title: this.newTask,
          description: "",
          date: "",
          done: false,
        });
        this.newTask = "";
      }
    },
  },
});
</script>
