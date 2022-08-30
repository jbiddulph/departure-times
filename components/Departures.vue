<template>
  <div class="flex flex-col">
    <div class="overflow-x-auto md:overflow-x">
      <table class="container mx-auto my-8">
        <thead class="text-left bg-gradient-to-r from-white to-light-gray my-7 rounded-lg p-4 text-sm md:text-lg border-0 mb-8">
          <th class="md:pl-20 rounded-l-lg py-4 px-2 mb-8">
            Departure Time
          </th>
          <th class="py-4 px-2">
            City Name
          </th>
          <th class="py-4 px-2">
            Code
          </th>
          <th class="py-4 px-2">
            Airline
          </th>
          <th class="py-4 px-2">
            Gate
          </th>
          <th class="py-4 px-2 rounded-r-lg pr-2">
            Status
          </th>
        </thead>
        <tbody class="rounded">
          <tr v-for="(departure, i) in allDepartures"
          @click.prevent="getSelectedDeparture(departure)"
          :key="departure.id"
          :index="i" class="content-center items-center row h-24 text-sm md:text-2xl border-white border-2 rounded-l-lg rounded-r-lg my-7 text-white font-bold">
            <td class="md:pl-20 p-4 my-7 px-2">{{ departure.scheduledDepartureDateTime.slice(-8, -3) }}</td>
            <td class="text-dark-yellow px-2">{{ departure.arrivalAirport.name.replace(" Airport", "") }}<br /><span class="text-light-yellow">{{ departure.arrivalAirport.countryName }}</span></td>
            <td class="px-2">{{ departure.arrivalAirport.code.toUpperCase() }}</td>
            <td class="px-2">{{ departure.airline.name }}</td>
            <td class="px-2">
              <!-- <div v-if="departure.departureGate.name.length">{{ departure.departureGate.name }}</div> -->
              <span class="text-dark-yellow">here</span>
            </td>
            <td class="rounded-r-lg px-2">
              <span class="border-l-8 border-solid border-light-yellow bg-white rounded-r-full flex text-black rounded-l-sm p-2 px-4 m:w-72">{{ departure.status }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <form method="POST">
      <h3 class="text-light-yellow">Departure Time: {{ this.selectedDeparture.departureTime }}</h3>
    </form>
  </div>
</template>

<script>
export default {
  name: "departures",
  props: {
    allDepartures: {
      typeof: 'array',
    }
  },
  data() {
    return {
      selectedDeparture: {
        departureTime: '',
        cityName: '',
        countryName: '',
        code: '',
        airline: '',
        gateNo: '',
        status: '',
      }
    }
  },
  methods: {
    getSelectedDeparture(departure) {
      this.selectedDeparture.departureTime = departure.scheduledDepartureDateTime.slice(-8, -3)
      this.selectedDeparture.cityName = departure.arrivalAirport.name
      this.selectedDeparture.countryName = departure.arrivalAirport.countryName
      this.selectedDeparture.code = departure.arrivalAirport.code.toUpperCase()
      this.selectedDeparture.airline = departure.airline.name
      this.selectedDeparture.gateNo = departure.status
      this.selectedDeparture.status = departure.status
      console.log('Selected Departure:', this.selectedDeparture)
    },
  }
}
</script>

<style>
tr {
  cursor: pointer;
}
</style>
