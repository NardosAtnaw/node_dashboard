<template>
  <v-form class="multi-col-validation">
    <v-row>
      <v-col cols="12">
        <v-select v-model="form.dayWeek" :items="days" label="Select Day of Schedule"></v-select>
      </v-col>
      <v-col cols="12" md="6">
        <h4>Class Start Time:</h4>
        <v-time-picker v-model="start" :max="end" format="ampm"></v-time-picker>
      </v-col>
      <v-col cols="12" md="6">
        <h4>Class End Time:</h4>
        <v-time-picker v-model="end" :min="start" format="ampm"></v-time-picker>
      </v-col>

      <v-col cols="12" md="6">
        <v-text-field v-model="form.className" label="Class Name" placeholder="Jazz,Ballet, Takewando "></v-text-field>
      </v-col>

      <v-col cols="12" md="6">
        <v-text-field v-model="form.studioNum" label="Studio Number" placeholder="1,2,3 "></v-text-field>
      </v-col>
      <v-col cols="12" md="6">
        <v-select v-model="form.ageGroup" :items="age" label="Select Age Range"></v-select>
      </v-col>

      <v-col cols="12">
        <v-textarea
          v-model="form.addInfo"
          name="input-7-1"
          label="Additional Information"
          placeholder="Twice a week/ Once a week "
          value=""
          hint="Hint text"
        ></v-textarea>
      </v-col>
      <v-col cols="12" md="6">
        <v-menu
          v-model="menu2"
          :close-on-content-click="false"
          :nudge-right="40"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field v-model="date" label="Select Start Date" readonly v-bind="attrs" v-on="on"></v-text-field>
          </template>
          <v-date-picker v-model="date" @input="menu2 = false"></v-date-picker>
        </v-menu>
      </v-col>
      <v-col cols="12" md="6">
        <v-menu
          ref="endMenu"
          v-model="endMenu"
          :close-on-content-click="false"
          :return-value.sync="form.endDate"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="form.endDate"
              label="Schedule End Date"
              readonly
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker v-model="form.endDate" no-title scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click="endMenu = false">
              Cancel
            </v-btn>
            <v-btn text color="primary" @click="$refs.endMenu.save(form.endDate)">
              OK
            </v-btn>
          </v-date-picker>
        </v-menu>
      </v-col>
      <v-col class="margin-btn" cols="12">
        <v-btn color="primary" @click.prevent="submitsch">
          Submit
        </v-btn>
      </v-col>
    </v-row>
  </v-form>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      // date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000).toISOString().substr(0, 10),
      // menu: false,
      // modal: false,
      // menu2: false,
      start: '',
      end: '',
      startMenu: false,
      endMenu: false,
      modal: false,
      menu2: false,
      age: ['3-5', '4-5', '6-9', '6-10', '10-18', '8+', '7+', '18+', 'all'],
      days: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
      form: {
        startDate: new Date(Date.now() - new Date().getTimezoneOffset() * 60000).toISOString().substr(0, 10),
        endDate: new Date(Date.now() - new Date().getTimezoneOffset() * 60000).toISOString().substr(0, 10),
        dayWeek: '',
        className: '',
        studioNum: '',
        sTime: '',
        eTime: '',
        ageGroup: '',
        addInfo: '',
      },
      time: null,
      menu2: false,
      modal2: false,
      time1: null,
      menu1: false,
      modal1: false,
    }
  },
  methods: {
    async submitsch() {
      await axios
        .post('https://www.beyonddancers.com/schedule', {
          form: this.form,
        })
        .then(res => {
          console.log(res.data)
        })

      // console.log(this.form.sTime)
    },
  },
  watch: {
    start(value) {
      this.form.sTime = value
      console.log('form', this.form)
    },
    end(value) {
      this.form.eTime = value
      console.log('end', this.form)
    },
  },
}
</script>
