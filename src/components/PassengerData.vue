<template>
  <div v-if="passenger && passenger.selectedFlight?.price" class="q-mt-lg q-px-lg rounded-borders q-card--bordered q-pa-sm">
    <strong class="text-h5">Thank you for booking your flight with us!</strong>
    <div class="q-mt-md">
      <strong>Passenger Information:</strong>
      <div class="q-ml-sm column">
        <span>
          First Name: {{ passenger.firstName }}
        </span>
        <span>
          Surname: {{ passenger.lastName }}
        </span>
        <span>
          Age: {{ passenger.age }}
        </span>
        <span>
          Email: {{ passenger.email }}
        </span>
        <span>
          Contact Number: {{ passenger.phone }}
        </span>
      </div>
    </div>
    <div class="q-mt-md">
      <strong>Flight Information:</strong>
      <div class="q-ml-sm column">
      <span>
        Departure Airport: {{ passenger.selectedFlight?.departureAirport }}
      </span>
        <span>
        Arrival Airport: {{ passenger.selectedFlight?.arrivalAirport }}
      </span>
        <span>
        Departure Date: {{ passenger.selectedFlight?.departureDate }}
      </span>
        <span>
        Departure Time: {{ passenger.selectedFlight?.departureTime }}
      </span>
        <span>
        Arrival Time: {{ passenger.selectedFlight?.arrivalTime }}
      </span>
        <span>
        Ticket Price: R{{ passenger.selectedFlight?.price }}
      </span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    passenger: {
      type: Object
    }
  }
}
</script>