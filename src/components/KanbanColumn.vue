<template>
  <v-col cols="4">
    <v-card class="pa-4">
      <v-card-title>{{ column.title }}</v-card-title>
      <v-btn color="primary" @click="openAddDialog">Tilføj kort</v-btn>

      <v-list class="mt-4">
        <KanbanCard
          v-for="card in column.cards"
          :key="card.id"
          :card="card"
          @edit="editCard"
          @delete="deleteCard"
        />
      </v-list>
    </v-card>
  </v-col>

  <!-- Dialog for at tilføje et nyt kort -->
  <v-dialog v-model="dialog" max-width="500px">
    <v-card>
      <v-card-title>Tilføj et nyt kort</v-card-title>
      <v-card-text>
        <v-text-field label="Titel" v-model="newCard.title" />
        <v-textarea label="Beskrivelse" v-model="newCard.description" />
      </v-card-text>
      <v-card-actions>
        <v-btn color="red" @click="dialog = false">Annuller</v-btn>
        <v-btn color="green" @click="addCard">Tilføj</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
// Import PropType using type-only import
import type { PropType } from "vue";
import KanbanCard from "./KanbanCard.vue";

// Define types for card and column
type Card = { id: number; title: string; description: string };
type Column = { id: number; title: string; cards: Card[] };

export default defineComponent({
  components: { KanbanCard },
  props: {
    column: {
      type: Object as PropType<Column>,
      required: true,
    },
  },
  setup(props, { emit }) {
    const dialog = ref(false);
    const newCard = ref({ title: "", description: "" });

    const openAddDialog = () => {
      dialog.value = true;
    };

    const addCard = () => {
      if (newCard.value.title && newCard.value.description) {
        emit("card-added", props.column.id, newCard.value.title, newCard.value.description);
        newCard.value = { title: "", description: "" };
        dialog.value = false;
      }
    };

    const editCard = (cardId: number, title: string, description: string) => {
      emit("card-updated", props.column.id, cardId, title, description);
    };

    const deleteCard = (cardId: number) => {
      emit("card-deleted", props.column.id, cardId);
    };

    return { dialog, newCard, openAddDialog, addCard, editCard, deleteCard };
  },
});
</script>
