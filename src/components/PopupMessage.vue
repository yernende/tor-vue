<template>
  <v-dialog :value="value" @input="$emit('input', false)" max-width="290">
    <v-card>
      <v-card-text>
        <slot />
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" flat="flat" @click="$emit('input', false)">Хорошо</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ["value"],
  name: "PopupMessage"
}
</script>
