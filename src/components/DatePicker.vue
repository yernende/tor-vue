<template>
  <v-menu
    ref="menu"
    :close-on-content-click="false"
    v-model="menu"
    :nudge-right="40"
    :return-value.sync="date"
    lazy
    transition="scale-transition"
    offset-y
    full-width
    min-width="290px"
  >
    <v-text-field
      slot="activator"
      v-model="date"
      :label="label"
      prepend-icon="event"
      :name="name"
      readonly
      :rules="rules"
    />
    <v-date-picker v-model="date" :max="max" :pickerDate.sync="pickerDate" no-title scrollable>
      <v-spacer />
      <v-btn flat color="primary" @click="menu = false">Cancel</v-btn>
      <v-btn flat color="primary" @click="$refs.menu.save(date); $emit('change', date)">OK</v-btn>
    </v-date-picker>
  </v-menu>
</template>

<script>
export default {
  name: "DatePicker",
  props: {
    label: String,
    name: String,
    max: String,
    rules: {
      type: Array,
      default: () => ([])
    }
  },
  data: function () {
    return {
      date: null,
      pickerDate: this.max ? this.max.slice(0, this.max.lastIndexOf("-")) : null,
      menu: false,
      modal: false
    }
  }
}
</script>
