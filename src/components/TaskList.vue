<script setup lang="ts">
import type Task from "../types";

const props = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();
</script>
<template>
  <TransitionGroup name="list" tag="div">
    <article v-for="task in tasks" class="task" :id="task.id" :key="task.id">
      <label>
        <input
          @input="emits('toggleDone', task.id)"
          type="checkbox"
          :checked="task.done"
        />
        <span :class="{ done: task.done }">
          {{ task.title }}
        </span>
      </label>
      <button @click="emits('removeTask', task.id)" class="outline">
        Remove
      </button>
    </article>
  </TransitionGroup>
</template>

<style>
.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.done {
  text-decoration: line-through;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
