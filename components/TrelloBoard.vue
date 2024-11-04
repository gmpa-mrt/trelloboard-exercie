<script setup lang="ts">
import type {Column, Task} from "@/types";
import {nanoid} from "nanoid";
import draggable from "vuedraggable";

/*
const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: "Backlog",
    tasks: [
      {
        id: nanoid(),
        title: "Creating marketing landing page",
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: "Develop cool new feature",
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: "Fix nav bug",
        createdAt: new Date()
      }
    ]
  },
  {
    id: nanoid(),
    title: "Selected for developer",
    tasks: []
  },
  {
    id: nanoid(),
    title: "In progress",
    tasks: []
  },
  {
    id: nanoid(),
    title: "QA",
    tasks: []
  },
  {
    id: nanoid(),
    title: "Complete",
    tasks: []
  }
])
*/

const columns = useLocalStorage<Column[]>("trelloBoard",[
  {
    id: nanoid(),
    title: "Backlog",
    tasks: [
      {
        id: nanoid(),
        title: "Creating marketing landing page",
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: "Develop cool new feature",
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: "Fix nav bug",
        createdAt: new Date()
      }
    ]
  },
  {
    id: nanoid(),
    title: "Selected for developer",
    tasks: []
  },
  {
    id: nanoid(),
    title: "In progress",
    tasks: []
  },
  {
    id: nanoid(),
    title: "QA",
    tasks: []
  },
  {
    id: nanoid(),
    title: "Complete",
    tasks: []
  }
])


const shift = useKeyModifier("Shift")

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: "",
    tasks: [],
  }
  columns.value.push(column)

  nextTick(() => {
    (document.querySelector('.column:last-of-type .title-input') as HTMLInputElement).focus()
  })
}

</script>

<template>
  <div class="flex items-start gap-4 overflow-x-auto">
    <draggable
        v-model="columns"
        group="columns"
        :animation="150"
        handle=".drag-handle"
        item-key="id"
        class="flex items-start gap-4"
    >
      <template #item="{element: column}: {element: Column}">
        <div :key="column.id"
             class="rounded bg-gray-200 p-5 column min-w-[250px]"
        >
          <header class="mb-4 font-bold">
            <DragHandle/>
            <input
                class="w-4/5 rounded bg-transparent px-1 title-input focus:bg-white"
                @keyup.enter="($event.target as HTMLInputElement).blur()"
                @keydown.backspace="column.title === '' 
                  ? columns = columns.filter(c => c.id !== column.id) 
                  : null"
                type="text"
                v-model="column.title"
            >
          </header>
          <draggable
              v-model="column.tasks"
              :group="{
                name: 'tasks',
                pull: shift ? 'clone' : true
              }"
              :animation="150"
              handle=".drag-handle"
              item-key="id"
          >
            <template #item="{element: task}: {element: Task}">
              <div>
                <TrelloBoardTask
                    :task="task"
                    @delete="column.tasks = column.tasks.filter(t => t.id !== $event)"
                />
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="column.tasks.push($event)"/>
          </footer>
        </div>
      </template>
    </draggable>
    <button
        @click="createColumn"
        class="whitespace-nowrap rounded bg-gray-200 px-2 opacity-50"
    >
      + Add new column
    </button>
  </div>
</template>