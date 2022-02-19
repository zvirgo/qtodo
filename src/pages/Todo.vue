<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add Task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered class="bg-white">
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ done: task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointer-events"
            v-model="task.done"
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
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon name="check" size="100px" />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "Todo",
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "Do Home Work",
          done: false,
        },
        {
          title: "Play Violin",
          done: true,
        },
        {
          title: "Read Book",
          done: false,
        },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Realy delete?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
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
.no-task{
  opacity: 0.5;
}
</style>
