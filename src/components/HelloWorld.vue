<template>
  <div class="q-pa-md content">
    <q-form
        @submit="searchFlights"
        class="q-gutter-md row items-start"
    >
      <q-input v-model="flightData.departureAirport" label="From"
               :rules="[ val => val && val.length > 0 || 'Please enter a valid departure airport']"/>
      <q-input v-model="flightData.arrivalAirport" label="To"
               :rules="[ val => val && val.length > 0 || 'Please enter a valid arrival airport.']"/>
      <q-input label="Departure" filled v-model="flightData.departureDate" mask="date"  style="width: 150px"
               :rules="[ val => new Date(val) >= new Date().setHours(0,0,0,0) || 'Please enter a valid date']">
        <template v-slot:append>
          <q-icon name="event" class="cursor-pointer">
            <q-popup-proxy cover transition-show="scale" transition-hide="scale">
              <q-date v-model="flightData.departureDate" minimal>
                <div class="row items-center justify-end">
                  <q-btn v-close-popup label="Close" color="primary" flat/>
                </div>
              </q-date>
            </q-popup-proxy>
          </q-icon>
        </template>
      </q-input>
      <q-input
          label="Travellers"
          v-model.number="flightData.travellers"
          type="number"
          filled
          style="max-width: 100px"
          :rules="[val => val && val >= 1]"
      />
      <q-btn color="primary" type="submit" style="width: 200px; height: 56px">
        <q-spinner-hourglass v-if="loading" class="on-left"/>
        <span v-else>Search Flights</span>
      </q-btn>
    </q-form>
    <div v-if="!passengerData || !passengerData.selectedFlight?.price">
      <AvailableFlights @confirmPassenger="confirmPassenger($event)" :flights="flights"/>
    </div>
    <PassengerData :passenger="passengerData"/>
  </div>
</template>

<script>
import {reactive, ref} from 'vue';
import AvailableFlights from './AvailableFlights'
import PassengerData from "./PassengerData";

export default {
  setup() {
    const formatDate = (date) => {
      const year = date.getFullYear();
      const month = date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth + 1;
      const day = date.getDate();

      return year + '/' + month + '/' + day
    }
    const initialState = {
      firstName: '',
      lastName: '',
      age: '',
      email: '',
      phone: '',
      selectedFlight: {}
    }
    const passengerData = reactive({...initialState})

    const loading = ref(false)

    const confirmPassenger = (e) => {
      console.log(e);
      Object.assign(passengerData, e);
    }

    const today = new Date();
    const flights = ref(null);
    //Simulating a request
    const searchFlights = () => {
      //reset state
      Object.assign(passengerData, initialState);
      flights.value = null;
      loading.value = true;
      setTimeout(() => {
        fetch('./flightList.json').then(resp => resp.json()).then(data => {
          flights.value = data.filter((flight) => {
            return flightData.departureAirport === flight.departureAirport && flightData.arrivalAirport === flight.arrivalAirport
                && flightData.departureDate === flight.departureDate
                && flightData.travellers <= flight.availableSeats
          })
          loading.value = false;
          console.log(flights.value)
        });
      }, 2000)
    }

    const flightData = reactive({
      departureAirport: '',
      arrivalAirport: '',
      departureTime: '',
      departureDate: formatDate(today),
      travellers: 1,
      selected: false
    });

    return {
      searchFlights,
      flightData,
      flights,
      passengerData,
      confirmPassenger,
      loading
    }
  },
  components: {
    PassengerData,
    AvailableFlights
  }
}
</script>
