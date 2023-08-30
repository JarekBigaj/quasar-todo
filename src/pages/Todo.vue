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
        dense
        placeholder="Add task"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        v-ripple
        @click="task.done = !task.done"
        clickable
        :class="{ done: task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primarry"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            color="primarry"
            dense
            icon="delete"
            @click.stop="deleteTask(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon name="check" size="100px" color="primarry" />
      <div class="div text-h5 text-primarry text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script lang="ts">
export default {
  name: 'ToDoList',
  data() {
    return {
      newTask: '',
      tasks: [] as { title: string; done: boolean }[],
    };
  },
  methods: {
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = '';
    },
    deleteTask(index: number) {
      this.$q
        .dialog({
          title: 'Confirm',
          message: 'Are you sure about that ?',
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify('Task deleted');
        });
    },
  },
};
</script>

<style lang="scss">
.done {
  background-color: #ef8a17;
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
