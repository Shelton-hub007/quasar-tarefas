<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        v-model="newTask"
        bg-color="white"
        filled
        dense
        placeholder="Add task"
        square
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, reactive, ref } from "vue";

export default defineComponent({
  name: "Todo",
  setup() {
    const newTask = ref("");
    const tasks = reactive([
      // {
      //   title: "Get Bananas",
      //   done: false,
      // },
      // {
      //   title: "Eat Bananas",
      //   done: false,
      // },
      // {
      //   title: "Poo Bananas",
      //   done: false,
      // },
    ]);

    return {
      tasks,
      newTask,
      deleteTask(index) {
        this.$q
          .dialog({
            title: "Confirm",
            message: "Really delete?",
            cancel: true,
            persistent: true,
          })
          .onOk(() => {
            this.tasks.splice(index, 1);
            this.$q.notify("Task Deleted");
          });
      },
      addTask() {
        tasks.push({ title: newTask.value, done: false });
        newTask.value = "";
      },
    };
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
