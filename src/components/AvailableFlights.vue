<template v-if="flights" class=" q-mt-lg">
  <div class="q-mt-lg q-px-lg rounded-borders q-card--bordered q-pa-sm" v-for="(flight, key) in filteredFlights"
       :key="key">
    <FlightInfo :disable="flightSelected" :flight="flight" @selectFlight="flight.selected = true; passengerData.selectedFlight = flight"/>
<!--    <div class="row full-width justify-between ">-->
<!--      <div class="column">-->
<!--        <span style="width: 100px" class="q-my-auto text-h6 text-weight-bold">{{ flight.departureTime }}</span>-->
<!--        <span style="width: 100px" class="q-my-auto">{{ flight.departureAirport }}</span>-->
<!--      </div>-->
<!--      <div class="column">-->
<!--        <span style="width: 100px" class="q-my-auto text-h6 text-weight-bold">{{ flight.arrivalTime }}</span>-->
<!--        <span style="width: 100px" class="q-my-auto">{{ flight.arrivalAirport }}</span>-->
<!--      </div>-->
<!--      <span style="width: 100px" class="q-my-auto text-h5 text-weight-bold">R {{ flight.price }}</span>-->
<!--      <q-btn @click="flight.selected = true; passengerData.selectedFlight = flight" :disabled="flightSelected" color="primary" style="width: 200px;">-->
<!--        Book Flight-->
<!--      </q-btn>-->
<!--    </div>-->
    <q-form
        v-if="flight.selected"
        class=" q-mt-md wrap"
    >
      <div class="row q-gutter-md">
        <q-input
            filled
            v-model="passengerData.firstName"
            label="First Name"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
        />
        <q-input
            filled
            v-model="passengerData.lastName"
            label="Surname"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
        />
        <q-input
            filled
            v-model="passengerData.age"
            type="number"
            label="Age"
            lazy-rules
            :rules="[
          val => val !== null && val !== '' || 'Please type your age',
          val => val > 0 && val < 100 || 'Please type a real age'
        ]"
        />
      </div>
      <div class="row q-gutter-md">
        <q-input
            filled
            v-model="passengerData.email"
            type="email"
            label="Email"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
        />
        <q-input
            filled
            type="phone"
            v-model="passengerData.phone"
            label="Phone number"
        />
      </div>
      <div class="q-gutter-md">
        <q-btn label="Confirm" @click="$emit('confirmPassenger', passengerData)" color="primary"/>
        <q-btn label="Cancel" color="negative" @click="$emit('confirmPassenger', {selectedFlight: {}}); flight.selected = false; passengerData.selectedFlight = {}" />
      </div>
    </q-form>
  </div>
  <div v-if="flights && flights.length === 0" class="q-mt-lg q-px-lg rounded-borders q-card--bordered q-pa-sm">Sorry no flights have been found.</div>
  <q-pagination
      class="q-pa-lg flex flex-center"
      v-if="flights && flights.length > 1"
      v-model="offset"
      :max="Math.ceil(flights.length / 5)"
  />
</template>

<script>
import {reactive, computed, ref} from 'vue';
import FlightInfo from "./FlightInfo";

export default {
  components: {FlightInfo},
  props: {
    flights: {
      type: Array
    }
  },
  emits: ['confirmPassenger'],
  setup(props) {
    const selectedFlight = ref({});
    const selectFlight = (flight) => {
      passengerData.selectedFlight = flight;
    }
    const flightSelected = computed(() => {
      return props.flights.find((flight) => {
        selectedFlight.value = flight;
        return flight.selected;
      })
    })

    const filteredFlights = computed(() => {
      const flights = props.flights;
      if (flights) {
        return flights.sort((a, b) => {
          console.log(a)
          return a.departureTime.localeCompare(b.departureTime);
        }).filter((item, key) => {
          return key < offset.value*5 && key >= ((offset.value - 1)*5)
        })
      }
      return null;
    })

    const offset = ref(1);

    const passengerData = reactive({
      firstName: '',
      lastName: '',
      age: '',
      email: '',
      phone: '',
      selectedFlight: {}
    })

    return {
      passengerData,
      flightSelected,
      offset,
      filteredFlights,
      selectFlight
    }
  }
}
</script>