<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <add-appointment class="mb-2 col-6" @add="addItem" />
      <appointment-list :appointments="appointments" @remove="removeItem" @edit="editItem" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';

import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import _ from 'lodash';
import AppointmentList from "./components/appointmentList";
import AddAppointment from './components/addAppointment';

export default {
  name: 'MainApp',
  data: function() {
    return {
      title: 'Appointment List',
      appointments: [],
      aptIndex: 0
    };
  },
  components: {
    FontAwesomeIcon,
    AppointmentList,
    AddAppointment
  },
  mounted() {
    axios
      .get('./data/appointments.json')
      .then(response => (this.appointments = response.data.map(item => {
        item.aptId = this.aptIndex;
        this.aptIndex++;
        return item;
      })))
  },
  methods: {
    removeItem: function(apt) {
      this.appointments = _.without(this.appointments, apt);
    },
    editItem: function(id, field, text) {
      const aptIndex = _.findIndex(this.appointments, {
        aptId: id
      });
      this.appointments[aptIndex][field] = text;
    },
    addItem: function(apt) {
      apt.aptId = this.aptIndex;
      this.aptIndex++;
      this.appointments.push(apt);
    }
  }
}
</script>
