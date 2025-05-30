<script setup lang="ts">
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import type { Task, TaskFilter } from "./types";
import FilterButton from "./components/FilterButton.vue";

const tasks = ref<Task[]>([]);

const filter = ref<TaskFilter>("all");

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0)
);

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "done":
      return tasks.value.filter((task) => task.done);
    case "todo":
      return tasks.value.filter((task) => !task.done);
  }
  return tasks.value;
});

function toggle(id: string) {
  const theTask = tasks.value.find((task) => task.id === id);
  if (theTask) theTask.done = !theTask.done;
}

function remove(id: string) {
  const index = tasks.value.findIndex((task) => task.id === id);
  if (index !== -1) tasks.value.splice(index, 1);
}

function setFilter(value: TaskFilter) {
  filter.value = value;
}
</script>

<template>
  <main>
    <h1>Tasks app</h1>
    <TaskForm @add-task="addTask" />
    <br />
    <h3 v-if="!tasks.length">Add a task to get started.</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} completed</h3>
    <div v-if="tasks.length" class="button-container">
      <FilterButton
        :currentFilter="filter"
        filter="all"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="todo"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="done"
        @set-filter="setFilter"
      />
    </div>
    <TaskList
      :tasks="filteredTasks"
      @toggleDone="toggle"
      @removeTask="remove"
    />
  </main>
</template>

<style>
main {
  max-width: 800px;
  margin: 1rem auto;
}

.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
  margin-bottom: 15px;
}
</style>
