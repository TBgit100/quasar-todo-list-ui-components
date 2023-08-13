<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        filled
        class="col"
        square
        bg-color="white"
        v-model="newTask"
        placeholder="add some task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered>
      <q-item
        @click="task.done = !task.done"
        clickable
        :class="{ 'done bg-green-2': task.done }"
        v-for="(task, index) in tasks"
        :key="task.title"
        tag="label"
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointers-events"
            v-model="task.done"
            val="orange"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            dense
            flat
            round
            color="primary"
            icon="remove_circle_outline"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon name="fact_check" size="150px" color="primary" />
      <div class="text-h4 text-primary">No Tasks!</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "hello",
        //   done: true,
        // },
        // {
        //   title: "hello2",
        //   done: false,
        // },
        // {
        //   title: "hello3",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm deleting",
          message: "Do you wanna really delete?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted!");
        });
    },
    addTask() {
      if (this.newTask != "") {
        this.tasks.push({
          title: this.newTask,
          done: false,
        });
        this.newTask = "";
      }
    },
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: grey;
  }
}

.no-task {
  opacity: 0.4;
}
</style>
