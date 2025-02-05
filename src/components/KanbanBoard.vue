<template>
  <v-container>
    <v-row>
      <KanbanColumn
        v-for="column in columns"
        :key="column.id"
        :column="column"
        @card-added="addCard"
        @card-updated="updateCard"
        @card-deleted="deleteCard"
      />
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import KanbanColumn from "./KanbanColumn.vue";

// Define types
type Card = { id: number; title: string; description: string };
type Column = { id: number; title: string; cards: Card[] };

export default defineComponent({
  components: {
    KanbanColumn,
  },
  setup() {
    const columns = ref<Column[]>([
      { id: 1, title: "To Do", cards: [] },
      { id: 2, title: "In Progress", cards: [] },
      { id: 3, title: "Done", cards: [] },
    ]);

    const addCard = (columnId: number, title: string, description: string) => {
      const column = columns.value.find((col) => col.id === columnId);
      if (column) {
        column.cards.push({ id: Date.now(), title, description });
      }
    };

    const updateCard = (columnId: number, cardId: number, title: string, description: string) => {
      const column = columns.value.find((col) => col.id === columnId);
      if (column) {
        const card = column.cards.find((c) => c.id === cardId);
        if (card) {
          card.title = title;
          card.description = description;
        }
      }
    };

    const deleteCard = (columnId: number, cardId: number) => {
      const column = columns.value.find((col) => col.id === columnId);
      if (column) {
        column.cards = column.cards.filter((c) => c.id !== cardId);
      }
    };

    return {
      columns,
      addCard,
      updateCard,
      deleteCard,
    };
  },
});
</script>
