<template>
  <v-app>
    <v-container>
      <h1 class="text-center my-4">Kanban Board</h1>
      <KanbanBoard />
    </v-container>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import KanbanBoard from "@/components/KanbanBoard.vue";

export default defineComponent({
  components: {
    KanbanBoard,
  },
});
</script>

<style scoped>
.kanban-card {
  border: 2px solid white !important;
}
</style>
