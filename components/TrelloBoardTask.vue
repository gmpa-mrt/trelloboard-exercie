<script setup lang="ts">
  import type {Task, ID} from "@/types";
  const props = defineProps<{
    task: Task
  }>()

  const emit = defineEmits<{
    (e: "delete", payload: ID): void
  }>()

  const focused = ref(false)

  onKeyStroke("Backspace", (e) => {
    if(focused.value) {
      emit("delete", props.task.id)
    }
  })
</script>

<template>
  <div :title="new Date(task.createdAt).toLocaleDateString()"
       class="task bg-white p-2 mb-2 rounded shadow-sm max-w-[250px] flex"
       @focus="focused = true"
       @blur="focused = false"
       tabindex="0"
  >
    <DragHandle class="pr-2"/>
    <span>{{ task.title}}</span>
  </div>
</template>

<style>
.sortable-drag .task{
  transform: rotate(5deg);
}

.sortable-ghost .task{
  position: relative;
}

.sortable-ghost .task::after {
  content: "";
  @apply absolute inset-0 bg-slate-300 rounded;
}

.task:focus,
.task:focus-visible {
  outline: gray auto 1px;
}
</style>