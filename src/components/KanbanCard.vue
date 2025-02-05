<template>
  <v-list-item>
    <v-list-item-content>
      <v-list-item-title>{{ card.title }}</v-list-item-title>
      <v-list-item-subtitle>{{ card.description }}</v-list-item-subtitle>
    </v-list-item-content>

    <v-btn icon color="blue" @click="openEditDialog">
      <v-icon>mdi-pencil</v-icon>
    </v-btn>
    <v-btn icon color="red" @click="$emit('delete', card.id)">
      <v-icon>mdi-delete</v-icon>
    </v-btn>
  </v-list-item>

  <!-- Dialog for editing a card -->
  <v-dialog v-model="editDialog" max-width="500px">
    <v-card>
      <v-card-title>Rediger kort</v-card-title>
      <v-card-text>
        <v-text-field label="Titel" v-model="editedCard.title" />
        <v-textarea label="Beskrivelse" v-model="editedCard.description" />
      </v-card-text>
      <v-card-actions>
        <v-btn color="red" @click="editDialog = false">Annuller</v-btn>
        <v-btn color="blue" @click="updateCard">Gem ændringer</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
// Import PropType using type-only import
import type { PropType } from "vue";

// Define types for card
type Card = {
  id: number;
  title: string;
  description: string;
};

export default defineComponent({
  props: {
    card: {
      type: Object as PropType<Card>,
      required: true,
    },
  },
  setup(props, { emit }) {
    const editDialog = ref(false);
    const editedCard = ref({ ...props.card });

    const openEditDialog = () => {
      editedCard.value = { ...props.card };
      editDialog.value = true;
    };

    const updateCard = () => {
      emit("edit", props.card.id, editedCard.value.title, editedCard.value.description);
      editDialog.value = false;
    };

    return { editDialog, editedCard, openEditDialog, updateCard };
  },
});
</script>
