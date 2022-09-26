<template>
  <div class="row full-width justify-between ">
    <div class="column">
      <span style="width: 100px" class="q-my-auto text-h6 text-weight-bold">{{ flight.departureTime }}</span>
      <span style="width: 100px" class="q-my-auto">{{ flight.departureAirport }}</span>
    </div>
    <div class="column">
      <span style="width: 100px" class="q-my-auto text-h6 text-weight-bold">{{ flight.arrivalTime }}</span>
      <span style="width: 100px" class="q-my-auto">{{ flight.arrivalAirport }}</span>
    </div>
    <span style="width: 100px" class="q-my-auto text-h5 text-weight-bold">R {{ flight.price }}</span>
    <q-btn @click="$emit('selectFlight')" :disabled="disable" color="primary" style="width: 200px;">
      Book Flight
    </q-btn>
  </div>
</template>

<script>

export default {
  props: {
    flight: {},
    disable: {
      type: Boolean
    }
  },
  emits: ['selectFlight']
}
</script>