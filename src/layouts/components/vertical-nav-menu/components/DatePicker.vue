<template>
  <v-container grid-list-md>
    <v-layout
      row
      wrap
    >
      <v-flex
        xs12
        lg6
      >
        <v-menu
          ref="menu1"
          v-model="menu1"
          :close-on-content-click="false"
          :nudge-right="40"
          lazy
          transition="scale-transition"
          offset-y
          full-width
          max-width="290px"
          min-width="290px"
        >
          <template v-slot:activator="{ on }">
            <v-text-field
              v-model="dateFormatted"
              label="Date"
              persistent-hint
              prepend-icon="mdi-calendar"
              @blur="$emit('update:date', parseDate(dateFormatted))"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-model="dateCopy"
            no-title
            @input="updateDate(date)"
          >
          </v-date-picker>
        </v-menu>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: 'DatePicker',
  props: {
    date: String,
  },
  data() {
    return {
      dateFormatted: this.formatDate(new Date().toISOString().substr(0, 10)),
      dateCopy: this.date,
      menu1: false,
    }
  },
  watch: {
    date(val) {
      this.dateFormatted = this.formatDate(this.dateCopy)
    },
  },
  methods: {
    formatDate(date) {
      if (!date) return null

      const [year, month, day] = date.split('-')

      return `${month}/${day}/${year}`
    },
    parseDate(date) {
      if (!date) return null

      const [month, day, year] = date.split('/')

      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    },
    updateDate(date) {
      this.menu1 = false
      this.$emit('update:date', this.dateCopy)
    },
  },
}
</script>
