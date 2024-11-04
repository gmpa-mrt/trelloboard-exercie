<script setup lang="ts">
  import type {Task} from "@/types";
  import {nanoid} from "nanoid";

  const title = ref("")
  const focused = ref(false)

  const emit = defineEmits<{
    (e: "add", payload: Task) : void
  }>()

  const createTask = (e: Event) => {
    if(title.value.trim()) {
      e.preventDefault()
      emit("add", {
        title: title.value.trim(),
        createdAt: new Date(),
        id: nanoid()
      } as Task)
    }
    // Reset
    title.value = ""
  }

</script>

<template>
  <textarea
    v-model="title"
    @keydown.tab="createTask"
    @keydown.enter="createTask"
    class="focus:bg-white focus:shadow resize-none rounded w-full bg-transparent p-2"
    :class="{
      'h-7': !focused,
      'h-20': focused
    }"
    style="outline: none !important;"
    @focus="focused = true"
    @blur="focused = false"
    :placeholder="!focused ? '+ Add a card' :  'Enter a title for this card'"
  >

  </textarea>
</template>

<style scoped>

</style>